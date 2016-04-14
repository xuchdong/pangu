# OpenGL：模版缓冲区


OpenGL的模版缓冲区其实就是采用过滤的技术来控制哪些颜色可以绘制，哪些颜色不进行绘制。主要体现在如下两个函数：
	
	glStencilFunc(GLenum func, GLint ref, GLuint mask)
	glStencilOp(GLenum fail, GLenum zfail, GLenum zpass)


完整的代码如下：

```
#include <stdio.h>
#include <math.h>
#include <OpenGL/OpenGL.h>
#include <OpenGL/glu.h>
#include <GLUT/GLUT.h>

void init()
{
    glClearColor(0.0, 0.0, 1.0, 0.0);
    glClearStencil(0);
    glEnable(GL_STENCIL_TEST);
}

void display()
{
    glClear(GL_COLOR_BUFFER_BIT|GL_DEPTH_BUFFER_BIT|GL_STENCIL_BUFFER_BIT);
    
    glLoadIdentity();
    glTranslatef(0.0, 0.0, -20);
    
    //glStencilFunc(GL_ALWAYS, 0, 0x00);
    glStencilFunc(GL_NEVER, 0x0, 0x0);
    glStencilOp(GL_INCR, GL_INCR, GL_INCR);
    
    glColor3f(1.0f, 1.0f, 1.0f);
    
    float dRadius = 5.8 * (sqrt(2.0) / 2.0);
    glBegin(GL_LINE_STRIP);
    for(float dAngle = 0; dAngle < 380.0; dAngle += 0.1)
    {
        glVertex2d(dRadius * cos(dAngle), dRadius * sin(dAngle));
        dRadius *= 1.003;
    }
    glEnd();
    
    glStencilFunc(GL_NOTEQUAL, 0X1, 0X1);
    glStencilOp(GL_INCR, GL_INCR, GL_INCR);
    
    glColor3f(1.0f, 0.0f, 0.0f);
    glRectf(-5, -5, 5, 5);
    
    glutSwapBuffers();
}

void reshape(int w, int h)
{
    glViewport(0, 0, w, h);
    float aspect = (w * 1.0)/h;
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(60, aspect, 1, 100);
    
    glMatrixMode(GL_MODELVIEW);
    glLoadIdentity();
}

int main(int argc, char* argv[])
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE|GLUT_RGBA|GLUT_STENCIL);
    glutInitWindowPosition(200, 200);
    glutInitWindowSize(600, 600);
    glutCreateWindow("stencil test");
    
    init();
    glutReshapeFunc(reshape);
    glutDisplayFunc(display);
    glutMainLoop();
    return 0;
}
```
