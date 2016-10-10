---
title: Save Content with Default Font
page_title: Save Content with Default Font | RadEditor for ASP.NET AJAX Documentation
description: Save RadEditor Content with Default Font
slug: editor/how-to/save-content-with-default-font
tags: save, content, with, default, font
published: True
position: 12
---

# Save Content with Default Font

In this article you will see how to save the RadEditor's content with a default font.

Generally, the editor's content is saved in a database without the external style sheets. This means only the inline styles will be preserved when the content is saved in a database. 

To define a default font for the editor content when it is submitted to the server you should use inline styles. You can use one of the following approaches:

* [Define a wrapper element with a particular font for the content area](#define-a-wrapper-element-with-a-particular-font-for-the-content-area)
* [Wrap the editor's content in an element with a desired font on submit](#wrap-the-editors-content-in-an-element-with-a-desired-font-on-submit)
* [Capture the enter key press over the editor content area in order and fire an editor's format command](#capture-the-enter-key-press-over-the-editor-content-area-in-order-and-fire-an-editors-format-command)
* [Define external stylesheets and use CSS inliner tool that will convert them to inline styles](#define-external-stylesheets-and-use-css-inliner-tool-that-will-convert-them-to-inline-styles)

## Define a wrapper element with a particular font for the content area

You can simply wrap the content of the editor inside an HTML element that has the desired inline font.

>caption Example 1: RadEditor content is wrapped inside an element with a particular font.

````ASP.NET
<telerik:RadEditor ID="RadEditor1" runat="server">
    <Content>
        <div style="font-family:Arial;font-size:20px;">
            some content in editor
        </div>
    </Content>
</telerik:RadEditor>
````

## Wrap the editor's content in an element with a desired font on submit

You can use the editor's [get_html()]({%slug editor/client-side-programming/methods/get_html%}) and [set_html()]({%slug editor/client-side-programming/methods/set_html%}) methods in order to get and set the content inside an element with a particular font before submit.

>caption Example 2: Wrap editor's content inside an element with a desired font on submit.

````ASP.NET
<script>
    function submitContent(sender, args) {
        var editor = $find("RadEditor2");
        editor.set_html("<div style='font-family:Arial;font-size:20px;'>" + editor.get_html() + "</div>");
    }
</script>
<telerik:RadButton ID="RadButton1" runat="server" Text="Submit Editor Content" AutoPostBack="true" OnClientClicked="submitContent" />
<telerik:RadEditor ID="RadEditor2" runat="server">
    <Content>
        some content in editor
    </Content>
</telerik:RadEditor>
````

## Capture the enter key press over the editor content area in order and fire an editor's format command

You can fire a particular font command (e.g., `FontName` or `RealFontSize`). This can be done via the toolbar or automatically by attaching to the [enter key press of the content area]({%slug editor/client-side-programming/events/onclientload%}).

>caption Example 3: Fire editor's font commands when pressing enter key in the content area.

````ASP.NET
<script>
    function OnClientLoad(editor, args) {
        editor.attachEventHandler("onkeydown", function (e) {
            if (e.keyCode == 13) {
                editor.fire("FontName", { value: "Arial" });
                editor.fire("RealFontSize", { value: "20px" });
            }
        });
    }
</script>
<telerik:RadEditor ID="RadEditor3" runat="server" OnClientLoad="OnClientLoad">
    <Content>
        some content in editor
    </Content>
</telerik:RadEditor>
````

## Define external stylesheets and use CSS inliner tool that will convert them to inline styles

You can use any external CSS Inliner Tool to convert the external stylesheets of the editor to inline styles. For example you can find below an integration sample with the [PreMailer.Net](https://github.com/milkshakesoftware/PreMailer.Net) CSS Inliner Tool.

1. Define the .css files via the CssFiles collection.
1. Read the .css files and place that content inside the style tag of the editor content.
1. Execute the CSS Inliner Tool over the modified content.

>caption Example 4: Use an external CSS Inliner Tool to convert external stylesheets to inline styles.

````CSS
/*ParagraphStylization.css*/
p {
    font-family: Arial;
    font-size: 20px;
}

````
````ASP.NET
<telerik:RadEditor runat="server" ID="RadEditor1">
    <CssFiles>
        <telerik:EditorCssFile Value="~/Styles/ParagraphStylization.css" />
    </CssFiles>
    <Content>
        <p>
            some content in paragraph
        </p>
    </Content>
</telerik:RadEditor>
````
````C#
protected void Button1_Click(object sender, EventArgs e)
{
    StringBuilder css = new StringBuilder();
    foreach (var item in RadEditor1.CssFiles)
    {
        EditorCssFile cssFile = (EditorCssFile)item;
        string path = Server.MapPath(cssFile.Value);
        css.Append(File.ReadAllText(path));
    }

    string content = RadEditor1.Content;

    string rawContent = "<html><head><style>" + css.ToString() + "</style></head><body>" + content + "</body></html>";

    var contentInlineStyles = PreMailer.Net.PreMailer.MoveCssInline(rawContent, true);
}
````
````VB
Protected Sub Button1_Click(sender As Object, e As EventArgs)
	Dim css As New StringBuilder()
	For Each item As var In RadEditor1.CssFiles
		Dim cssFile As EditorCssFile = DirectCast(item, EditorCssFile)
		Dim path As String = Server.MapPath(cssFile.Value)
		css.Append(File.ReadAllText(path))
	Next

	Dim content As String = RadEditor1.Content

	Dim rawContent As String = "<html><head><style>" & css.ToString() & "</style></head><body>" & content & "</body></html>"

	Dim contentInlineStyles = PreMailer.Net.PreMailer.MoveCssInline(rawContent, True)
End Sub
````

## See Also

* [RadEditor OnClientLoad event]({%slug editor/client-side-programming/events/onclientload%})
* [RadEditor get_html() method]({%slug editor/client-side-programming/methods/get_html%})
* [RadEditor set_html() method]({%slug editor/client-side-programming/methods/set_html%})
* [PreMailer.Net](https://github.com/milkshakesoftware/PreMailer.Net)
