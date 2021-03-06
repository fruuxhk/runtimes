---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Erforderliche Dateien in den Buildausgabeordner kopieren
{: #copy_files_build_output}

Sie können entweder project.json-Tools oder MSBuild-Tools verwenden um sicherzustellen, dass Ihre Anwendung über alle erforderlichen Dateien im Buildausgabeordner verfügt.
{: #shortdesc}


## project.json-Tools verwenden

Fügen Sie die folgende Eigenschaft zum Abschnitt `buildOptions` in der Datei 'project.json' hinzu:
```
  "copyToOutput": {
    "include": [
      "wwwroot",
      "Areas/**/Views",
      "Views",
      "appsettings.json"
    ]
  }
```
{: codeblock}

Entfernen Sie folgende Zeile in der `Startup`-Methode Startup.cs:
```
  .SetBasePath(env.ContentRootPath)
```
{: codeblock}

Entfernen Sie folgende Zeile in der `Main`-Methode Program.cs:
```
  .UseContentRoot(Directory.GetCurrentDirectory())
```
{: codeblock}

Diese Änderungen sollte der .NET-CLI ermöglichen, die `Sichten` Ihrer Anwendung zu finden, da diese jetzt in die Buildausgabe kopiert werden, wenn der Befehl `dotnet run` ausgeführt wird.  Wenn Ihre Anwendung über andere Dateien wie beispielsweise json-Konfigurationsdateien verfügt, die zur Laufzeit erforderlich sind, dann sollten Sie auch diese zum Abschnitt `include` von `copyToOutput` in der Datei 'project.json' für Ihr Projekt hinzufügen.

## MSBuild-Tools verwenden

Fügen Sie ein Element `<Content>` zum Element `<ItemGroup>` Ihrer .csproj-Datei hinzu:
```
  <ItemGroup>
    <Content Include="wwwroot/**/*;Areas/**/Views/*;Views/*;appsettings.json">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
      <CopyToPublishDirectory>Always</CopyToPublishDirectory>
    </Content>
  </ItemGroup>
```
{: codeblock}

Entfernen Sie folgende Zeile in der `Startup`-Methode Startup.cs:
```
  .SetBasePath(env.ContentRootPath)
```
{: codeblock}

Entfernen Sie folgende Zeile in der `Main`-Methode Program.cs:
```
  .UseContentRoot(Directory.GetCurrentDirectory())
```
{: codeblock}

Diese Änderungen sollte der .NET-CLI ermöglichen, die `Sichten` Ihrer Anwendung zu finden, da diese jetzt in die Buildausgabe kopiert werden, wenn der Befehl `dotnet publish` ausgeführt wird.  Wenn Ihre Anwendung über andere Dateien wie beispielsweise json-Konfigurationsdateien verfügt, die zur Laufzeit erforderlich sind, dann sollten Sie auch diese durch Semikolons getrennt zur Eigenschaft `Include` des Elements `Content` in der .csproj-Datei Ihres Projekts hinzufügen.
