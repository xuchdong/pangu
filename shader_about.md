#### Shader About

使用步骤：

- 创建shader
- 创建program
- 使用program
- 绑定属性（在glsl 330以后使用in out, 不再使用attribute变量）
- 激活

Fall Example:

```
GLuint createShader(GLenum type, const char* src)
{
    GLuint shader = glCreateShader(type);
    glShaderSource(shader, 1, &src, NULL);
    
    glCompileShader(shader);
    
    GLint status;
    glGetShaderiv(shader, GL_COMPILE_STATUS, &status);
    if (status == GL_FALSE) {
        GLint infoLogLen = 0;
        glGetShaderiv(shader, GL_INFO_LOG_LENGTH, &infoLogLen);
        
        char* infoLog = (char *)malloc(sizeof(char) * infoLogLen);
        glGetShaderInfoLog(shader, infoLogLen, NULL, infoLog);
        printf("%s", infoLog);
        free(infoLog);
        infoLog = NULL;
        //glDeleteShader(shader);
        return 0;
    }
    return shader;
}

GLuint createProgram(GLuint* shaderList, int len)
{
    GLuint program = glCreateProgram();
    for (int i = 0; i < len; i++) {
        glAttachShader(program, *(shaderList + i));
    }
    glLinkProgram(program);
    
    GLint status;
    glGetProgramiv(program, GL_LINK_STATUS, &status);
    if (status == GL_FALSE) {
        GLint infoLogLen = 0;
        glGetProgramiv(program, GL_INFO_LOG_LENGTH, &infoLogLen);
        
        char *infoLog = (char *)malloc(sizeof(char)*infoLogLen);
        glGetProgramInfoLog(program, infoLogLen, NULL, infoLog);
        free(infoLog);
        infoLog = NULL;
        //glDeleteProgram(program);
        return 0;
    }
    for (int i = 0; i < len; i++) {
        glDetachShader(program, *(shaderList + i));
    }
    
    return program;
}


static char *vert = "                                           \n\
attribute vec4 a_position;                                      \n\
void main()                                                     \n\
{                                                               \n\
    gl_Position = a_position + vec4(0.0, -0.25, 0.0, 0.0);      \n\
}                                                               \n\
";

static char *freg = "                                           \n\
void main()                                                     \n\
{                                                               \n\
    float lerpValue = gl_FragCoord.y / 100.0;                   \n\
                                                                \n\
    gl_FragColor = mix(vec4(1.0, 1.0, 1.0, 1.0),                \n\
                      vec4(0.2, 0.2, 0.2, 1.0), lerpValue);     \n\
}                                                               \n\
";


static GLuint program = 0;

void init()
{
    glClearColor(0.0, 0.0, 0.0, 0.0);
    glShadeModel(GL_FLAT);
    
    GLuint vertShader = createShader(GL_VERTEX_SHADER, vert);
    GLuint fregShader = createShader(GL_FRAGMENT_SHADER, freg);
    
    GLuint shaderList[] = {vertShader, fregShader};
    program = createProgram(shaderList, 2);
    
    glBindAttribLocation(program, 1, "a_position");
    glUseProgram(program);
}

static GLfloat vertices[] = {
        -0.75, -0.75, 0.0,
        0.0, 0.75, 0.0,
        0.75, -0.75, 0.0
};

void display()
{
    glClear(GL_COLOR_BUFFER_BIT);
    glColor3f(1.0, 0.0, 0.0);
    glEnableClientState(GL_VERTEX_ARRAY);
    GLuint index = glGetAttribLocation(program, "a_position");
    //glEnableVertexAttribArray(index);
    //glVertexPointer(3, GL_FLOAT, 0, vertices);
    glEnableVertexAttribArray(index);
    glVertexAttribPointer(index, 3, GL_FLOAT, GL_FALSE, 0, vertices);
    glDrawArrays(GL_TRIANGLES, 0, 3);
    
    glFlush();
}

```