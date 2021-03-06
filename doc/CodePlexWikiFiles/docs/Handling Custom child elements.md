# Handling Custom child elements

Each Configuration Element has a property called _Has Custom Child Elements_. This tells the tool that this element may contain some child configuration elements (XML nodes) that vary and are not known at design-time.

Setting this flag causes the tool to generate an override for ConfigurationElement's [OnDeserializeUnrecognizedElement](http___msdn.microsoft.com_en-us_library_system.configuration.configurationelement.ondeserializeunrecognizedelement.aspx) method. In this overridden method, a call to another method is made, _HandleUnrecognizedElement_. This method is where you can do your custom child element handling. This method is not implemented, though, and will cause the compiler to complain. Instructions on how to properly create this method is described in the comments of the generated code.

In addition to generating this code, it also indicates in the generated XML Schema that the configuration element can contain any kind of element. This is done in order to let the configuration section be valid regardless of the contents of the configuration element.

## Use case example

Suppose your configuration has a logger element and that a logger is instantiated through reflection in your application. That logger instance then parses the inner XML to define its settings.

{code:xml}
<logger type="Logging.FileLogger, CustomLoggingAssembly">
  <fileOptions filename="c:\log.txt" buffered="true" />
</logger>
{code:xml}

The <fileOptions> tag is something that only the FileLogger class would know about. One might as well put in another logger type that does not use the <fileOptions>, but instead use <databaseOptions>. Clearly, the child element is custom _depending on the logger type_. We cannot decide on how this section's child elements work at design-time.