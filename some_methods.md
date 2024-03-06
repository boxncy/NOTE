#### 生成随机图像

```cpp
include <random>
{
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<> dis(0, 255);

	// 图像尺寸
	const int width = 1280;
	const int height = 1280;

	// 图像数量
	const int imageCount = 1500;

	// 保存图像的路径
	const QString path = "e:/path/save/images";

	QDir dir(path);
	if (!dir.exists()) {
	    dir.mkpath(".");
	}

	// 生成并保存图像
	for (int i = 0; i < imageCount; ++i) {
	    // 创建图像
	    QImage image(width, height, QImage::Format_RGB32);
	    for (int x = 0; x < width; ++x) {
	        for (int y = 0; y < height; ++y) {
	            const int r = dis(gen);
	            const int g = dis(gen);
	            const int b = dis(gen);
	            image.setPixel(x, y, qRgb(r, g, b));
	        }
	    }

	    // 保存图像
	    const QString fileName = QString("%1/image_%2.bmp").arg(path).arg(i);
	    image.save(fileName,"bmp",100);
	    qDebug() << "Saved image: " << fileName;
	}
```

#### 生成各个格式灰度彩色图

```cpp
#include "direct.h"
#include <io.h>
void draw_mat(int height ,int width ,std::string name, std::string suffix){
    const std::string path = "e:/path/save/cv_images";
    const std::string save_name = path + "/" + name + "." + suffix;
    const std::string gray_save_name = path + "/"  + name + "_gray" + "." + suffix;
    if (_access(path.c_str(), 0) == -1)
    {
        _mkdir(path.c_str());
    }

    cv::Mat image(height,width,CV_8UC3);
    cv::rectangle(image,cv::Rect(0,0,width,height),cv::Scalar(0,0,255),2);
    cv::putText(image,suffix,cv::Point(width/2,height/2),cv::FONT_HERSHEY_SIMPLEX,1.0, cv::Scalar(255, 255, 255),2);
    cv::imwrite(save_name,image);
    cv::Mat gray_image;
    cv::cvtColor(image, gray_image, cv::COLOR_BGR2GRAY);
    cv::imwrite(gray_save_name, gray_image);
}
```



#### 判断图片文件头

```cpp
auto handle_import_images = [this](const QString& image_path ,const QString& source_path ,const cv::Mat& picture){
        if(!picture.empty()){
            QStringList supported_formats = {"png", "bmp", "jpg"};

            std::ifstream file(QSTR_TO_LOCAL(image_path), std::ios::binary);
            if (!file.good()) {
                return;
            }

//            or,form cv::mat
//            unsigned char *data = picture.ptr<unsigned char>(0, 0);
            unsigned char header[8];
            file.read(reinterpret_cast<char*>(header), 8);
            if (file.gcount() < sizeof(header)) {
                return;
            }
//            std::stringstream header_hex;
//            for(int i = 0; i < sizeof(header); i++){
//                header_hex << std::hex << std::setfill('0') << std::setw(2)
//                           << static_cast<int>(header[i]) << " ";
//            }
//            qDebug() << "Header as hexadecimal:" << header_hex.str().c_str();

            QString extension;
            if (header[0] == 0x42 && header[1] == 0x4D) {
                extension = "bmp";
            } else if (header[0] == 0x89 && header[1] == 0x50 && header[2] == 0x4E && header[3] == 0x47
                       && header[4] == 0x0D && header[5] == 0x0A && header[6] == 0x1A && header[7] == 0x0A) {
                extension = "png";
            } else if (header[0] == 0xFF && header[1] == 0xD8) {
                extension = "jpg";
            } else {
                return;
            }
//            const QString extension = QFileInfo(image_path).suffix().toLower();
            if(supported_formats.contains(extension)){
                if(this->operator_info().project_type == aq::EAQProjectType::EAQProjectPNG && extension == "bmp"){
                    this->futures_.append(QtConcurrent::run(process_image, picture, source_path, std::ref(this->mutex_)));
                }else{
                    QFile::copy(image_path, source_path);
                }
            }
        }
    };
```

#### 复制文件

```cpp
auto copy_file = [](const QString& source_path ,const QString& target_path){
                QFileInfo target_model(target_path);
                if(target_model.exists())
                {
                    QFile target_model_file(target_model.absoluteFilePath());
                    target_model_file.remove();
                }
                QFile::copy(source_path, target_model.absoluteFilePath());
            };


```

#### 修改qt json

```cpp
void modify_json(QJsonObject &obj ,QList<QString>list ,const QJsonValue &new_value){
    QString str = list.takeFirst();
    if(str.isEmpty()){
        return;
    }
    if(!obj.contains(str)){
        return;
    }
    if(list.size() == 0){
        obj[str] = new_value;
        return;
    }
    QJsonObject nextDestObj = obj[str].toObject();
    modify_json(nextDestObj ,list ,new_value);
    obj[str] = nextDestObj;
}

```

#### 分割增量中替换指定字段和叶子节点

```cpp
void replaceFields(QJsonObject &baseObj, const QJsonObject &updateObj)
{
    for (auto it = updateObj.begin(); it != updateObj.end(); it++) {
        if (it.value().isObject() && baseObj.contains(it.key())) {
            QJsonObject nextDestObj = baseObj[it.key()].toObject();
            replaceFields(nextDestObj, it.value().toObject());
            baseObj[it.key()] = nextDestObj;
            if ((it.key() == "split_height") || (it.key() == "split_width") || (it.key() == "input_shape")) {
                baseObj[it.key()] = it.value();
            }
        } else if (baseObj.contains(it.key())) {
            if (it.key() == "value") {
                baseObj[it.key()] = it.value();
//                if(baseObj["permission"].toInt() != 2){
//                    baseObj[it.key()] = it.value();
//                }
            }
        }
    }
}

```

```
git checkout -b new_branch
git add -u
git push -u origin fix()6_17bugs
```

