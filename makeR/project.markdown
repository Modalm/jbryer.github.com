---
layout: default
title: makeR
subtitle: An R package for managing document templates and versions
submenu: default
---

### Project Structure

The `makeR` project uses an object oriented framework where all interaction occurs through a single class, `Project`. The constructor method (i.e. R function) has a number of parameters.

* `projectDir` - the root directory of the project. This is where the `PROJECT.xml` file which contains the project information allowing for projects to persist between R sessions. The default is the current working directory.
* `name` - the name of the project. This is optional and only used when creating a new project.
* `sourceDir` - the relative path to the directory containing the source files. This is only used when creating a new project.
* `buildDir` - the relative path to the directory containing the build files. This is only used when creating a new project.
* `releaseDir` - the relative path to the directory containing the released files. This is only used when creating a new project.
* `sourceFile` - the name or pattern for the source file(s) to be built. If not specified the builder used will determine the appropriate files to build.
* `properties` - list of project properties.

The following example from the `rbloggers` demo demonstrates how to create a new project.

	myProject = Project(name="RBloggers", projectDir=projectDir,
			properties=list(email=email, passwd=passwd))

Subsequent R sessions require only specifying the `projectDir` parameter.

	myProject = Project(projectDir=projectDir)

#### Attributes

* `BuildDir` - the directory where builds will occur.
* `Builds` - list of completed builds.
* `CurrentBuild` - an integer of the last build.
* `ProjectDir` - the base directory where the project is located.
* `ProjectFile` - the name of the source file to be built.
* `ProjectName` - the name of the project.
* `Properties` - a list of the project properties. See also \code{p$getProperties()}.
* `ReleaseDir` - the directory where released files will be located.
* `SourceDir` - the directory containing the source files.
* `Versions` - a list of the project versions.

#### Methods

* `build` - Builds the project.
	* `version` - (optional) the version to build.
	* `saveEnv` - if TRUE, the build environment (.rda) will be saved in the build directory.
* `builder` - the builder function. See also `builder.rnw`, `builder.cacheSweave`, `builder.tex`
* `rebuild` - Rebuilds the project without first copying the files.
	* `version` - (optional) the version to rebuild.
	* `saveEnv` - if TRUE, the build environment (.rda) will be saved in the build directory.
	* `builder` - the builder function. See also `builder.rnw`, `builder.cacheSweave`, `builder.tex`
* `save` - Saves the PROJECT.xml file.
* `newVersion` Creates a new versions of the project.
	* `name` - (optional) the version name.
	* `properties` - version specific properties.
* `release` - Releases a version (i.e. copies the built file to the releases directory)
		
	* `version` - (optional) the version to release. If omitted the latest version will be released.
* `getProperties` - Returns the project properties.
* `addProperty` - Adds a project property.
	* `name` - The property name.
	* `value` - The property value.
* `removeProperty` - Removes the given project property.
	* `name` - The property name.
* `getReleases` - Returns a list of released files.
* `openRelease` - Opens the given released file with the system's default application.
	* `file` - The released file to open.
