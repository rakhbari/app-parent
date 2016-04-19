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
 
### Git Build Info File
This parent POM uses the [`maven-git-commit-id-plugin`](https://github.com/ktoso/maven-git-commit-id-plugin "git-commit-id-plugin"). Please refer to that plugin's github page for full instructions.

But in short, you include a `git.properties` as a template in your project's `/src/main/resources` directory and this plugin generates a file of the exact same name with all the available git & build time info under your project's `/target/classes`.

You can then read that `git.properties` file at runtime and provide a `/admin/build-info` (or something similar) to your users.
