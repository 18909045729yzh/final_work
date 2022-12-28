#   计算机图形学大作业
##  如何加载 obj 模型 
这里我们通过创建了一个 model 类在其中实现 obj 模型的加载和展示
```
class Objmodel {
private:
 //obj模型信息
 vector< vector<GLfloat>> vertex;
 vector< vector<GLfloat>> vertex_texture;
vector< vector<GLfloat>> vertex_normal;
//以下三种分别是不同类型的f开头数据类型
 vector< vector<int>> face_vertex; 
 vector< vector<int>> face_texture;
 vector< vector<int>> face_normal;
 GLint v_num;
 GLint vt_num;
 GLint vn_num;
 GLint f_num;
 //保存obj边界点，计算模型包围盒
 vector<GLfloat > center;
 vector<GLfloat > one0fcatercorner; //包围盒对角线中一个点
 vector<GLfloat > other0fcatercorner; //包围盒对角线中另一个点
public:
 Objmodel();
 ~Objmodel();
 //从文件中读取数据
 void readFile(string path);
 //显示模型数据
 void showObj(GLint mode1, GLint mode2, GLuint texture[]);
//将中心平移到原点
//这一步是为了在查看模型时模型能够出现在中央位置
 vector<GLfloat> getCenter();
 //计算模型顶点法向
 void calculateNormal();
};
```
其中的代码具体实现见cpp文件以及pdf中
##   显示obj模型和纹理
```
void Objmodel::showObj(GLint mode1, GLint mode2, GLuint texture[]) {
 glEnable(GL_TEXTURE_2D);
 glPolygonMode(GL_FRONT_AND_BACK, mode1);
 glBindTexture(GL_TEXTURE_2D, texture[0]);
 glBegin(mode2);
 for (int i = 0; i < f_num; i++)
{
 glTexCoord3f(vertex_texture[face_normal[i][0] - 1][0], 
vertex_texture[face_normal[i][0] - 1][1], vertex_texture[face_normal[i][0] - 1][2]);
 glNormal3f(vertex_normal[face_normal[i][0] - 1][0], 
vertex_normal[face_normal[i][0] - 1][1], vertex_normal[face_normal[i][0] - 1][2]);
 glVertex3f(vertex[face_vertex[i][0] - 1][0], vertex[face_vertex[i][0] - 1][1], 
vertex[face_vertex[i][0] - 1][2]);
 glTexCoord3f(vertex_texture[face_normal[i][1] - 1][0], 
vertex_texture[face_normal[i][1] - 1][1], vertex_texture[face_normal[i][1] - 1][2]);
 glNormal3f(vertex_normal[face_normal[i][1] - 1][0], 
vertex_normal[face_normal[i][1] - 1][1], vertex_normal[face_normal[i][1] - 1][2]);
 glVertex3f(vertex[face_vertex[i][1] - 1][0], vertex[face_vertex[i][1] - 1][1], 
vertex[face_vertex[i][1] - 1][2]);
 glTexCoord3f(vertex_texture[face_normal[i][2] - 1][0], 
vertex_texture[face_normal[i][2] - 1][1], vertex_texture[face_normal[i][2] - 1][2]);
 glNormal3f(vertex_normal[face_normal[i][2] - 1][0], 
vertex_normal[face_normal[i][2] - 1][1], vertex_normal[face_normal[i][2] - 1][2]);
 glVertex3f(vertex[face_vertex[i][2] - 1][0], vertex[face_vertex[i][2] - 1][1], 
vertex[face_vertex[i][2] - 1][2]);
 }
 glEnd();
 glDisable(GL_TEXTURE_2D);
}
```
##  读取和加载纹理
```
static unsigned char* LoadBmpFile(char* filename, BITMAPINFOHEADER* bmpInfoHeader)
{
FILE* filePtr = fopen(filename, "rb"); //打开BMP文件
if (filePtr == NULL) { //文件打开错误
printf("Texture file open error.\n");
return NULL;
}
BITMAPFILEHEADER bmpFileHeader;
fread(&bmpFileHeader, sizeof(BITMAPFILEHEADER), 1, filePtr); // 读入BMP文件头
if (bmpFileHeader.bfType != 0x4D42) { // 验证是否为bitmap文件
fprintf(stderr, "The file is not a bitmap file.\n");
return NULL;
}
fread(bmpInfoHeader, sizeof(BITMAPINFOHEADER), 1, filePtr); // 读入BMP信息头
fseek(filePtr, bmpFileHeader.bfOffBits, SEEK_SET); // 将文件指针移至BMP数据
区 
unsigned char* bmpImage = new unsigned char[bmpInfoHeader->biSizeImage]; //存放
位图数据
if (!bmpImage) { //内存创建失败
fprintf(stderr, "Error in LoadBitmapFile: memory error\n");
return NULL;
}
fread(bmpImage, 1, bmpInfoHeader->biSizeImage, filePtr);
unsigned char temp;
for (int idx = 0; idx < bmpInfoHeader->biSizeImage; idx += 3) { //将BGR转换为
RGB
temp = bmpImage[idx];
bmpImage[idx] = bmpImage[idx + 2];
bmpImage[idx + 2] = temp;
}
fclose(filePtr);
return bmpImage;
}
//加载纹理
void load(GLuint* text)
{
char filenames[6][20];
sprintf(filenames[0], "wave.bmp");
sprintf(filenames[1], "top.bmp");
sprintf(filenames[2], "left.bmp");
sprintf(filenames[3], "front.bmp");
sprintf(filenames[4], "right.bmp");
sprintf(filenames[5], "back.bmp");
for (int i = 0; i < 6; i++) {
BITMAPINFOHEADER bmpInfoHeader;
unsigned char* bmpData = LoadBmpFile(filenames[i], &bmpInfoHeader);
glBindTexture(GL_TEXTURE_2D, text[i]);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
glTexImage2D(GL_TEXTURE_2D,
0,
GL_RGB,
bmpInfoHeader.biWidth,
bmpInfoHeader.biHeight,
0,
GL_RGB,
GL_UNSIGNED_BYTE,
bmpData);
}
}
```
