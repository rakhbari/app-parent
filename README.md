# app-parent
Minimal parent project for any Java app (or service) that needs to be packaged in a fat-jar (using Maven shade plugin). It also produced a build info file so your code can report on its build time info.

### Build & Use
To build this project do this:

	$ mvn clean install

To use it just place the following at the top of your own `pom.xml`:

	<parent>
		<groupId>org.public</groupId>
		<artifactId>app-parent</artifactId>
		<version>1.0.0</version>
	</parent>

Once you've done this, every time you build your app will end up as a far JAR under your project's `/target` directory. Please read below about your app's main class.

### Your App's Main Class
You have to explicitly define your app's main class (fully qualified) in your app's `pom.xml` inside a `<properties>` section:

	<app.main.class>com.mycompany.myapp.MyAppMain</app.main.class>

This is the property key this project's shaded JAR plugin will use to set the `Main-Class:` attribute in your app's JAR MANIFEST.MF file.

__NOTE__: Failure to do this will leave your app's fat JAR un-executable!
 
### Build Info File
This is a file with default name `scm-info.txt` that's generated automatically every time you build your project. Its name can be overridden with the following snippet inside a `<properties>` section of your own `pom.xml` file:

	<scm.info.file>my-build-info.txt</scm.info.file>

With this file you can write a minimal processor class in your app to read this file and then provide a REST endpoint, such as `/admin/build-info`, to return the info stored in this file, which might look like this:

	{
	  "branch": "master",
	  "version": "1.3.0",
	  "commit": "2f3c0163a2dd4a1743f120adca975449e2daa481",
	  "lastChangedAuthor": "Your Name <email@mycompany.com>",
	  "buildDate": 1460758203427,
	  "buildDateString": "2016-04-15 15:10:03.427 -0700",
	  "buildDateShortString": "20160415-151003PDT"
	}
