# Gear-Simulation
This package “GEAR SIMULATION” is to simulate simple movement and rotation of a gear. A 3D Gear will be rotating on a z-axis, we can use keys w, a, s, d for the movement of the gear and arrow keys for rotation of the gear on x, y, z-axis.

#include <math.h>
#include <stdlib.h>
#include <GL/glut.h>
#define M_PI 3.14159265

static GLfloat universalScale=0.2;
static GLint frontCleared=0;
static GLfloat gearColor[4]={1.0,0.0,0.0,1.0};
static GLfloat view_rotx = 20.0, view_roty = 30.0, view_rotz = 0.0;
static GLint gear1;
static GLfloat angle = 0.0;
static GLint limit;
static GLint count = 1;
static GLfloat transx=0.0;
static GLfloat transy=0.0;
static GLfloat transz=0.0;
static GLfloat rotAngle=0.1;
void main_menu(int index);


static void gear(GLfloat inner_radius, GLfloat outer_radius, GLfloat width,GLint teeth, GLfloat tooth_depth)
{
  GLint i;
  GLfloat r0, r1, r2;
  GLfloat angle, da;
  GLfloat u, v, len;

  r0 = inner_radius;
  r1 = outer_radius - tooth_depth / 2.0;
  r2 = outer_radius + tooth_depth / 2.0;

  da = 2.0 * M_PI / teeth / 4.0;

  glShadeModel(GL_FLAT);

  glNormal3f(0.0, 0.0, 1.0);

  glBegin(GL_QUAD_STRIP);
  for (i = 0; i <= teeth; i++) {
    angle = i * 2.0 * M_PI / teeth;
    glVertex3f(r0 * cos(angle), r0 * sin(angle), width * 0.5);
    glVertex3f(r1 * cos(angle), r1 * sin(angle), width * 0.5);
    glVertex3f(r0 * cos(angle), r0 * sin(angle), width * 0.5);
    glVertex3f(r1 * cos(angle + 3 * da), r1 * sin(angle + 3 * da), width * 0.5);
  }
  glEnd();


  glBegin(GL_QUADS);
  da = 2.0 * M_PI / teeth / 4.0;
  for (i = 0; i < teeth; i++) {
    angle = i * 2.0 * M_PI / teeth;

    glVertex3f(r1 * cos(angle), r1 * sin(angle), width * 0.5);
    glVertex3f(r2 * cos(angle + da), r2 * sin(angle + da), width * 0.5);
    glVertex3f(r2 * cos(angle + 2 * da), r2 * sin(angle + 2 * da), width * 0.5);
    glVertex3f(r1 * cos(angle + 3 * da), r1 * sin(angle + 3 * da), width * 0.5);
  }
  glEnd();

  glNormal3f(0.0, 0.0, -1.0);


  glBegin(GL_QUAD_STRIP);
  for (i = 0; i <= teeth; i++) {
    angle = i * 2.0 * M_PI / teeth;
    glVertex3f(r1 * cos(angle), r1 * sin(angle), -width * 0.5);
    glVertex3f(r0 * cos(angle), r0 * sin(angle), -width * 0.5);
    glVertex3f(r1 * cos(angle + 3 * da), r1 * sin(angle + 3 * da), -width * 0.5);
    glVertex3f(r0 * cos(angle), r0 * sin(angle), -width * 0.5);
  }
  glEnd();


  glBegin(GL_QUADS);
  da = 2.0 * M_PI / teeth / 4.0;
  for (i = 0; i < teeth; i++) {
    angle = i * 2.0 * M_PI / teeth;

    glVertex3f(r1 * cos(angle + 3 * da), r1 * sin(angle + 3 * da), -width * 0.5);
    glVertex3f(r2 * cos(angle + 2 * da), r2 * sin(angle + 2 * da), -width * 0.5);
    glVertex3f(r2 * cos(angle + da), r2 * sin(angle + da), -width * 0.5);
    glVertex3f(r1 * cos(angle), r1 * sin(angle), -width * 0.5);
  }
  glEnd();


  glBegin(GL_QUAD_STRIP);
  for (i = 0; i < teeth; i++) {
    angle = i * 2.0 * M_PI / teeth;

    glVertex3f(r1 * cos(angle), r1 * sin(angle), width * 0.5);
    glVertex3f(r1 * cos(angle), r1 * sin(angle), -width * 0.5);
    u = r2 * cos(angle + da) - r1 * cos(angle);
    v = r2 * sin(angle + da) - r1 * sin(angle);
    len = sqrt(u * u + v * v);
    u /= len;
    v /= len;
    glNormal3f(v, -u, 0.0);
    glVertex3f(r2 * cos(angle + da), r2 * sin(angle + da), width * 0.5);
    glVertex3f(r2 * cos(angle + da), r2 * sin(angle + da), -width * 0.5);
    glNormal3f(cos(angle), sin(angle), 0.0);
    glVertex3f(r2 * cos(angle + 2 * da), r2 * sin(angle + 2 * da), width * 0.5);
    glVertex3f(r2 * cos(angle + 2 * da), r2 * sin(angle + 2 * da), -width * 0.5);
    u = r1 * cos(angle + 3 * da) - r2 * cos(angle + 2 * da);
    v = r1 * sin(angle + 3 * da) - r2 * sin(angle + 2 * da);
    glNormal3f(v, -u, 0.0);
    glVertex3f(r1 * cos(angle + 3 * da), r1 * sin(angle + 3 * da), width * 0.5);
    glVertex3f(r1 * cos(angle + 3 * da), r1 * sin(angle + 3 * da), -width * 0.5);
    glNormal3f(cos(angle), sin(angle), 0.0);
  }

  glVertex3f(r1 * cos(0), r1 * sin(0), width * 0.5);
  glVertex3f(r1 * cos(0), r1 * sin(0), -width * 0.5);

  glEnd();

  glShadeModel(GL_SMOOTH);


  glBegin(GL_QUAD_STRIP);
  for (i = 0; i <= teeth; i++) {
    angle = i * 2.0 * M_PI / teeth;

    glNormal3f(-cos(angle), -sin(angle), 0.0);
    glVertex3f(r0 * cos(angle), r0 * sin(angle), -width * 0.5);
    glVertex3f(r0 * cos(angle), r0 * sin(angle), width * 0.5);
  }
  glEnd();

}


static void display(void)
{
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glPushMatrix();
    glTranslatef(transx,transy,transz);
    glRotatef(view_rotx, 1.0, 0.0, 0.0);
    glRotatef(view_roty, 0.0, 1.0, 0.0);
    glRotatef(view_rotz, 0.0, 0.0, 1.0);

    glPushMatrix();
    glTranslatef(0.0, 0.0, 0.0);
    glScalef(universalScale,universalScale,universalScale);
    glRotatef(angle, 0.0, 0.0, 1.0);
    glCallList(gear1);
    glPopMatrix();

    glPopMatrix();


    glutSwapBuffers();


}


static void idle(void)
{
    angle += rotAngle;
    glutPostRedisplay();
}


static void key(unsigned char k, int x, int y)
{
  switch (k) {
  case 'z':
        view_rotz += 5.0;
    break;
  case 'x':
        view_rotz -= 5.0;
    break;
  case 'w':
    transy+=0.1;
    if(transy>0.9)
        transy=-0.9;
    break;
    case 's':
        transy-=0.1;
        if(transy<-0.9)
        transy=0.9;
    break;
  case 'a':
    transx-=0.1;
    if(transx<-1.3)
        transx=1.3;
    break;
  case 'd':
    transx+=0.1;
    if(transx>1.3)
        transx=-1.3;
    break;

  case 'h':
        transz+=0.1;
    break;
  case 'j':
        transz-=0.1;
    break;
  default:
    return;

  }

  glutPostRedisplay();
}


static void special(int k, int x, int y)
{
  switch (k) {
  case GLUT_KEY_UP:
        view_rotx += 5.0;
    break;
  case GLUT_KEY_DOWN:
        view_rotx -= 5.0;
    break;
  case GLUT_KEY_LEFT:
        view_roty += 5.0;
    break;
  case GLUT_KEY_RIGHT:
        view_roty -= 5.0;
    break;
  default:
    return;
  }
  glutPostRedisplay();
}


static void reshape(int width, int height)
{
    GLfloat h = (GLfloat) height / (GLfloat) width;
    glViewport(0, 0, (GLint) width, (GLint) height);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    glOrtho(-1.0, 1.0, -h, h, 5.0, 60.0);
    glMatrixMode(GL_MODELVIEW);
    glLoadIdentity();
    glTranslatef(0.0, 0.0, -40.0);

}


static void init(void)
{
    static GLfloat pos[4] =
    {5.0, 5.0, 10.0, 1.0};

    static GLfloat mt_shin[]={200.0};
    glLightfv(GL_LIGHT0, GL_POSITION, pos);
    glEnable(GL_CULL_FACE);
    glEnable(GL_LIGHTING);
    glEnable(GL_LIGHT0);
    glEnable(GL_DEPTH_TEST);
    glShadeModel(GL_SMOOTH);

    gear1 = glGenLists(1);
    glNewList(gear1, GL_COMPILE);
    glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, gearColor);
    glMaterialfv(GL_FRONT,GL_SHININESS,mt_shin);
    gear(0.5, 1.5, 0.2, 20, 0.2);
    glEndList();

    glEnable(GL_NORMALIZE);
}


void visible(int vis)
{
  if (vis == GLUT_VISIBLE)
    glutIdleFunc(idle);
  else
    glutIdleFunc(NULL);
}


void draw_text(float x,float y,char *s)
{
    int i;
    glRasterPos2f(x,y);
    for(i=0;s[i]!='\0';i++)
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24,s[i]);
}


void front()
{
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glClearColor(0.6,0.84,0.9,0.0);
    glColor3f(0.0,0.0,1.0);
    draw_text(-0.4,0.45,"BANGALORE INSTITUTE OF TECHNOLOGY");
    draw_text(-0.39,0.4,"Department of Computer Science & Engineering ");
    draw_text(-0.4,0.33,"COMPUTER GRAPHICS AND VISULIZATION ");
    draw_text(-0.2,0.0,"Package on:");
    draw_text(-0.2,-0.05,"GEAR STIMULATION ");
    draw_text(-0.9,-0.3,"Submitted by:                                                             		                                                  Under the guidance of:");
    draw_text(-0.9,-0.35,"SUCHIT R                                              	                                                                      Prof.Dr.M.Kempanna");
    draw_text(-0.9,-0.39,"1BI14CS159                             		                                                                                    Prof.Kavitha.K");
    draw_text(-0.9,-0.43,"CSC-C(C2)                                                      		                                                            Dept.of CSE,BIT");
    glutSwapBuffers();
}


void menu()
{
    int GearColorMenu,scaleMenu,rotspeedMenu,primaryMenu;
        GearColorMenu=glutCreateMenu(main_menu);
            glutAddMenuEntry("White",11);
            glutAddMenuEntry("Yellow",12);
            glutAddMenuEntry("Violet",13);
            glutAddMenuEntry("Green",14);
            glutAddMenuEntry("Blue",15);
        scaleMenu=glutCreateMenu(main_menu);
            glutAddMenuEntry("Increase",8);
            glutAddMenuEntry("Normal",9);
            glutAddMenuEntry("Decrease",10);
        rotspeedMenu=glutCreateMenu(main_menu);
            glutAddMenuEntry("1x",5);
            glutAddMenuEntry("2x",6);
            glutAddMenuEntry("3x",7);


        primaryMenu = glutCreateMenu(main_menu);
            glutAddSubMenu("Scale", scaleMenu);
            glutAddSubMenu("RotateSpeed",rotspeedMenu);
            glutAddSubMenu("GearColor", GearColorMenu);
            glutAddMenuEntry("Exit",16);
        glutAttachMenu(GLUT_RIGHT_BUTTON);
}


void mouse(int btn, int state, int x, int y)
{
		if (btn == GLUT_LEFT_BUTTON && state == GLUT_DOWN)
		{
			glutPostRedisplay();
		}
}


void main_menu(int index)
{
    switch(index)
    {
    case 5:
            rotAngle=0.2;
        break;
    case 6:
            rotAngle=0.5;
        break;
    case 7:
            rotAngle=1.0;
        break;

    case 8:
            universalScale=0.35;
        break;
    case 9:
            universalScale=0.2;
        break;
    case 10:
            universalScale=0.09;
        break;
    case 11:
            gearColor[0]=1.0;
            gearColor[1]=1.0;
            gearColor[2]=1.0;
            gearColor[3]=1.0;
            init();

        break;
    case 12:
            gearColor[0]=0.9;
            gearColor[1]=0.9;
            gearColor[2]=0.1;
            gearColor[3]=1.0;
            init();

        break;
    case 13:
            gearColor[0]=0.63;
            gearColor[1]=0.28;
            gearColor[2]=0.64;
            gearColor[3]=1.0;
            init();

        break;
    case 14:
            gearColor[0]=0.0;
            gearColor[1]=1.0;
            gearColor[2]=0.0;
            gearColor[3]=1.0;
            init();

        break;
    case 15:
            gearColor[0]=0.0;
            gearColor[1]=0.0;
            gearColor[2]=1.0;
            gearColor[3]=1.0;
            init();

        break;

    case 16:
            exit(0);
        break;
    case 100:
            glClearColor(0.0,0.0,0.0,0.0);
            glClear(GL_COLOR_BUFFER_BIT);
            init();
            menu();
            glutDisplayFunc(display);
            glutReshapeFunc(reshape);
            glutKeyboardFunc(key);
            glutSpecialFunc(special);
            glutVisibilityFunc(visible);
            glutMainLoop();
            break;

    }
}


void menu1()
{
    glutCreateMenu(main_menu);
    glutAddMenuEntry("Start",100);
    glutAttachMenu(GLUT_RIGHT_BUTTON);

}


int main(int argc, char *argv[])
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_RGB | GLUT_DEPTH | GLUT_DOUBLE);
    glutInitWindowPosition(0,0);
    glutInitWindowSize(1200,700);
    glutCreateWindow("GEAR STIMULATION");
    menu1();
    glutDisplayFunc(front);
    glutReshapeFunc(reshape);
    glutSpecialFunc(special);
    glutVisibilityFunc(visible);
    glutMainLoop();

  return 0;
}


