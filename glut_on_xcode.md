# How to Use GLUT for OpenGL on Mac OSX Using XCode

- Install Xcode through the Apple App Store.

- Open Xcode and create a new project. Under OSX select Application, then Command Line Tool.

- Fill in some information, like the Product Name, and ensure the Type drop down is set to C. Then click Next. Choose a folder to save the project in.

- Click the project in the top-left of the sidebar, and then click Build Phases. Click ‘Link Binary With Libraries’ to expand it.

- Click the + button, and type ‘OpenGL’ in the search box. Click OpenGL.framework, then click Add.

- Click the + button again, and type ‘glut’ in the search box. Click GLUT.framework, then click Add. You are now linking with the appropriate frameworks at compile time.

- Now click the Build Settings tab. In the search bar in the top right of the tab, type ‘deprecated’.

- Under Apple LLVM – Warnings – All Languages, you should see one a setting for “Deprecated Functions”. Change the setting to “No”. This will keep Xcode from highlighting every GLUT OpenGL command you execute in yellow, because it treats all deprecated functions as warnings unless you turn that feature off, which is super annoying.

- In the sidebar, select main.c

- After the include for stdio.h, include these files:

		#include <OpenGL/gl.h>
		#include <OpenGL/glu.h>
		#include <GLUT/glut.h>

	This includes the necessary GLUT and OpenGL libraries into your project (the libraries are included with OSX by default, no need to download them from any website).

- Write your OpenGL code in the body of main.c

- Run the program, and it should build and execute fine.