### Getting the archetype
Download the archetype from [here](https://github.com/devonfw/oasp4j-template-client-server) and save it to Devonfw distribution's _workspaces/main_ folder.

### Installing the archetype
Open the Devonfw console by executing the batch file console.bat from the Devonfw distribution. It is a pre-configured console which automatically uses the software and configuration provided by the Devonfw distribution.  

In the console, Go to <root_of_the_directory>/templates and run the following command to install the archetpye  

_mvn clean install_

If everything goes fine, you'll see the results in the console as below

![Archetype Installation Sucessful](images/devon4sencha/sencha-client-archetype/archetype_install_success.png)

### Creation of Sencha Client Application

In Eclipse, go to _File_ --> _New_ --> _Maven Project_.  
A dialog box will open, click _Next_. On this screen, select checkbox _Include snapshot archetype_ and search for Artifact Id _**oasp4j-template-client-server**_. Select the archetype and click _Next_.

![Archetype Search](images/devon4sencha/sencha-client-archetype/archetype_search.png)

Enter _Group Id_ and _Artifact Id_ for your project and click _Finish_.  

A new project will be created in Eclipse with structure as shown below

![Project Structure](images/devon4sencha/sencha-client-archetype/project_modules.png)


### Setup Client module

By Default, auto validation of Javascript files is enabled in Eclipse. For client module it is time saving to set it to manual since it will take long time to validate all javascript files, every time build is triggered. To do the same follow the steps below.

1. Right click the client module(in our example it is SenchaServerApp-client) and select _Properties_.
2. A new window will open. In the left panel click on _validation_ as shown below   

![Validation Menu](images/devon4sencha/sencha-client-archetype/project_validation.png)

3. In the right panel select _Enable project specific settings_ as show below

![Enable Project Specific Validation](images/devon4sencha/sencha-client-archetype/project_validation_enable.png)

4. Unselect checkbox for _Javascript Validation_ under _Build_ column as shown below

![Unselect Validation on Build](images/devon4sencha/sencha-client-archetype/project_validation_checkbox.png)

5. Click _Apply_ and then _OK_.


