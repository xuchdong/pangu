### Texture About

#### 纹理贴图的步骤

- 创建纹理对象，并为它指定一个纹理
	
	```
GLuint texture;
glGenTextures( 1, &texture );
	```

- 确定纹理如何应用到每个像素上
	
	```
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);  
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);  
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST);  
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);  
glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, imageRec->sizeX, imageRec->sizeX,   
                            0, GL_RGB, GL_UNSIGNED_BYTE, imageRec->data)
```
- 启用纹理贴图功能并选择纹理。

	```
glEnable( GL_TEXTURE_2D );
glBindTexture( GL_TEXTURE_2D, texture );
	```

- 绘制场景，提供纹理坐标和几何图形坐标

	```
glBegin( GL_QUADS );
glTexCoord2d(0.0,0.0); glVertex2d(0.0,0.0);
glTexCoord2d(1.0,0.0); glVertex2d(1.0,0.0);
glTexCoord2d(1.0,1.0); glVertex2d(1.0,1.0);
glTexCoord2d(0.0,1.0); glVertex2d(0.0,1.0);
glEnd();
	```