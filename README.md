#   计算机图形学大作业
##  如何加载 obj 模型 
这里我们通过创建了一个 model 类在其中实现 obj 模型的加载和展示
···class Objmodel {
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
};···
