## Video

[![screen shot 2015-12-17 at 7 42 49 pm](https://cloud.githubusercontent.com/assets/1182323/11888779/72454a3a-a4f6-11e5-9907-725518ad7c02.png)](https://www.youtube.com/watch?v=Ml03yALid10)

## Step by Step

1. Go to http://www.eclipse.org/downloads/ and download "Eclipse IDE for Java Developers" for your platform. When the download completes, unzip or install Eclipse and start it up.
2. Eclipse will ask you where to store it's workspace. The default is fine. You can click "Use this as the default and do not ask again." if you don't want to be bothered every time you start Eclipse.
3. When Eclipse starts you will see a Welcome screen. Close the Welcome screen by clicking the small X in the tab.
4. You should now see the Package Explorer tab. Right click in this area and select Import.
5. On the Import screen, expand "Git", select "Projects from Git" and click Next.
6. On the Select Repository Source screen select "Clone URI" and click Next.
7. On the Source Git Repository screen enter `https://github.com/openpnp/openpnp.git` in the URI field<sup>1</sup> or enter the address to your fork at Github if you would like to contribute back to the project later<sup>2</sup> and click Next.
8. On the Branch Selection screen select the "develop" branch and unselect all other branches, then click Next.
9. On the Local Destination screen enter the directory where you would like to store the OpenPnP source code. You can keep the default if it's okay. Then click Next.
10. Eclipse will now download the OpenPnP source code. This may take a few minutes.
11. When it finishes, on the same screen select the "Import as general project" option and click Next.
12. On the Import Projects screen accept the default project name of "openpnp" and click Finish.
13. The openpnp project will now appear in the Package Explorer. Right click on the project "openpnp" and in the menu select Configure -> Convert to Maven Project. Eclipse will download any resources that it needs and build the project. This can take some time and you need to wait for it to complete before proceeding to the next step. 
14. Expand the openpnp project and look for the `src/main/java folder`. Under this folder expand the `org.openpnp` folder and look for `Main.java`.
15. Right click `Main.java` and from the menu select Run As -> Java Application.
16. After a few seconds OpenPnP will start.
17. You can now edit files in Eclipse, the changes will automatically be hot swapped if enabled "Project" -> "Build Automatically". If function signatures were changed hot swap does not apply and you need to rerun OpenPnP with your changes, just click the green Run button in the toolbar. The button is a green circle with a white right facing arrow in it.
18. Close OpenPnP
* <sup>1</sup> It is possible to later change so your clone is pointing to your fork at Github.
* <sup>2</sup> You cannot directly push your changes to OpenPnP, it must go through your own fork.

### Working with different configuration directories

When launching OpenPNP from within Eclipse IDE it will use your default configuration directory.
If you want to use an own directory, for example if you are using different branches with different machine.xml, then you can supply the **configDir** variable in the "Run Configuration":

![eclipse_configDir](https://user-images.githubusercontent.com/11256235/90951519-ff069600-e45b-11ea-90e3-b6bf13bd6caf.png)

* Select menu "Run -> Run configurations"
* Select your Main Java Application settings
* Choose the Arguments tab
* In the section VM arguments add: -DconfigDir=yourdirectory


## Additional steps if you want to contribute
1. Ensure you have the automatic code formatter for eclipse activated. See the [coding style-section in the developers guide to download a preconfigured settings file](https://github.com/openpnp/openpnp/wiki/Developers-Guide#coding-style).
2. You need to run tests by executing "Maven test". Maven test needs the full JDK + GIT be installed (not only JRE).
    * Download and install the JDK.
    * Download and install GIT.
5. Add the JDK to the environments in eclipse:
![jdk-env](https://user-images.githubusercontent.com/3868450/51134165-166cae80-1837-11e9-933e-2a6fbf1301ac.PNG)

