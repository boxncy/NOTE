```
AQROIRectObject
```

```
void AQPostDealGraphicsRectItem::stretch
void AQItemIndexPanel::slt_item_changed
load_pixmap_item
```

```
QListWidgetItem *next_item = lst_itemindex_->get_next_item();
        if (lst_itemindex_->currentRow() == -1 && next_item)
            lst_itemindex_->setCurrentItem(next_item);
```

```
AQLabelConvertor::convert_label //xml json label
```

应用场景:在原有模型及训练集的基础上，增加更多图片进行训练(每次新增图片应少于

原训练集20%)启用此模式可大幅减少训练时间。



没有新增图片：增量训练会在原有模型基础上继续训练，相比诚信训练模型，检测准确率可

能会有微量下降。



暂不支持：训练集原有图片变化，包括图片移出训练集，删减图片，修改ROI，掩膜，不

学习区域，标注（增删改），标签（增删改）。启用后，可修改参数仅包括：迭代次数，

训练批次，GPU



分类 ：label_hash,需要比对的部分

1. enum dataset_type **必须比对**
2. cv::Size img_size **必须比对**
3. MultiPolygon masks **必须比对**
4. std::vector<Region> regions (**仅比对**regions.polygon.name)

regions字段需要比对的部分

1. string name
2. polygon **无**

```c++
#include <../AQBase/aq_file_helper.h>
#include <core/aidi_types.h>
#include "utils/image_utils.h"

QTime* mt = new QTime();
        mt->start();
        QString label_path = QString("D:/PROTECT/aaaaaa/test/Classify_3/label/2.aqlabel") ;
        aq::in::Label label;
        std::string result;
        float hash_time=0;
        float for_time=0;
        for(int i =0;i<20000;i++) {
            if(i%1000 == 0){
                qDebug()<<"iter"<<i<<" time:"<<mt->elapsed()/1000.0;
            }

            if(AQFileHelper::is_file_exist(label_path) && aq::aidi::LabelIO::ParseFrom(aidi::gbk_from_unicode(label_path), label)){
                std::string hash_dataset_type = std::string("dateset_info") + std::to_string(label.dataset_type);
                cv::Size img_size = label.img_size;
                std::string hash_img_size =std::string("img_size") +std::to_string(img_size.width)+std::to_string(img_size.height);
                std::string hash_str_mask;

                QTime af;
                af.start();
                for(int k = 0; k < label.masks.size(); k++){
                    const aq::in::Polygon& masks = label.masks[k];
                    hash_str_mask.append("mask_inner");
                    for(int j = 0; j< masks.inners().size(); j++){
                        const aq::in::Ring& ring = masks.inners()[j];
                        if(ring.size() > 0){
                            for(const cv::Point2f& point : ring){
                                hash_str_mask.append(std::to_string(point.x));
                                hash_str_mask.append(std::to_string(point.y));

                            }
                        }
                    }
                    const aq::in::Ring& outer_ring = masks.outer();
                    hash_str_mask.append("mask_outer");
                    if(outer_ring.size() > 0){
                        for(const cv::Point2f& point : outer_ring) {
                            hash_str_mask.append(std::to_string(point.x));
                            hash_str_mask.append(std::to_string(point.y));
                        }
                    }
                }
//                qDebug()<<"str"<<QString::fromStdString(hash_str_mask);
                for_time+=af.elapsed()/1000.0;
                if(i%1000==0){
                     qDebug()<<"masks_time"<<for_time;
                }
                std::string hash_name("name");
                if(label.regions.size()> 0){
                    hash_name.append(label.regions[0].name);
                }
                result.append(hash_dataset_type);
                result.append(hash_img_size);
                result.append(hash_name);
                result.append(hash_str_mask);
    //            std::string std_result = result.toStdString();
                QTime ad;
                ad.start();
                std::string hash_single_label = aq::hash_md5(result.data(), result.size());
                hash_time+=ad.elapsed()/1000.0;
                if(i%1000==0){
                    qDebug()<<"hash_time"<<hash_time;
                }
            }
        }
        qDebug()<<"time:"<<mt->elapsed()/1000.0<<"s";
```

```c++
QTime* mt = new QTime();
    mt->start();
    QString label_path = QString("D:/PROTECT/aaaaaa/test/Classify_3/label/2.aqlabel") ;
    aq::in::Label label;
    std::string result;
    for(int i =0;i<20000;i++) {
        if(i%1000 == 0){
           qDebug()<<"iter"<<i<<"time"<<mt->elapsed()/1000.0<<"s";
        }

        if(AQFileHelper::is_file_exist(label_path) && aq::aidi::LabelIO::ParseFrom(aidi::gbk_from_unicode(label_path), label)){
            aq::aidi::LabelIO label_io;
            label_io.set_data(label);
            QJsonDocument doc = QJsonDocument::fromJson(label_io.to_json().data());
            //data_set_info
            std::string hash_dataset_type = std::string("dateset_info") + std::to_string(label.dataset_type);
            //img_size
            cv::Size img_size = label.img_size;
            std::string hash_img_size =std::string("img_size") +std::to_string(img_size.width)+std::to_string(img_size.height);
            //masks
            std::string hash_str_mask;
            QJsonDocument jsonDoc;
            jsonDoc.setArray(doc["masks"].toArray());
            hash_str_mask = jsonDoc.toJson(QJsonDocument::Compact);
            //label_name
            std::string hash_name("name");
            if(label.regions.size()> 0){
                hash_name.append(label.regions[0].name);
            }
            //result
            result.append(hash_dataset_type);
            result.append(hash_img_size);
            result.append(hash_name);
            result.append(hash_str_mask);
            std::string hash_single_label = aq::hash_md5(result.data(), result.size());
        }
    }
    qDebug()<<"time:"<<mt->elapsed()/1000.0<<"s";
```

```c++
QTime* mt = new QTime();
    mt->start();
    QString label_path = QString("D:/PROTECT/aaaaaa/test/Classify_3/label/2.aqlabel") ;
    aq::in::Label label;
    for(int i =0;i<20000;i++) {
        std::string result;
        if(i%10000 == 0){
            qDebug()<<"iter"<<i<<" time:"<<mt->elapsed()/1000.0;
        }
        if(AQFileHelper::is_file_exist(label_path) && aq::aidi::LabelIO::ParseFrom(aidi::gbk_from_unicode(label_path), label)){

            //dataset_type
            std::string hash_dataset_type = std::string("dateset_info") + std::to_string(label.dataset_type);

            //img_size
            cv::Size img_size = label.img_size;
            std::string hash_img_size =std::string("img_size") +std::to_string(img_size.width)+std::to_string(img_size.height);

            //masks
            std::vector<float> inner_x;
            std::vector<float> inner_y;
            std::vector<float> outer_x;
            std::vector<float> outer_y;
            const int masks_size = label.masks.size();
            for(int i = 0; i < masks_size; i++){
                const aq::in::Polygon& masks = label.masks[i];
                int inner_size = masks.inners().size();
                for(int j = 0; j< inner_size; j++){
                    const aq::in::Ring& ring = masks.inners()[j];
                    if(ring.size() > 0){
                        for(const cv::Point2f& point : masks.inners()[j]){
                            inner_x.push_back(point.x);
                            inner_y.push_back(point.y);
                        }
                    }
                }
                const aq::in::Ring& outer_ring = masks.outer();
                if(outer_ring.size() > 0){
                    for(const cv::Point2f& point : masks.outer()) {
                        outer_x.push_back(point.x);
                        outer_y.push_back(point.y);
                    }
                }
            }
            std::string str_inner_x;
            std::string str_inner_y;
            std::string str_outer_x;
            std::string str_outer_y;
            std::string hash_str_mask;
            hash_str_mask.append("mask_inner");
            hash_str_mask.append(str_inner_x.assign(inner_x.begin(),inner_x.end()));
            hash_str_mask.append(str_inner_y.assign(inner_y.begin(),inner_y.end()));
            hash_str_mask.append("mask_outer");
            hash_str_mask.append(str_outer_x.assign(outer_x.begin(),outer_x.end()));
            hash_str_mask.append(str_outer_y.assign(outer_y.begin(),outer_y.end()));

            //name
            std::string hash_name("name");
            if(label.regions.size()> 0){
                hash_name.append(label.regions[0].name);
            }
            result.append(hash_dataset_type);
            result.append(hash_img_size);
            result.append(hash_name);
            result.append(hash_str_mask);
            std::string hash_single_label = aq::hash_md5(result.data(), result.size());
            if(i%10000==0){
                qDebug()<<"hash"<<QString::fromStdString(hash_single_label);
            }
        }
    }
    qDebug()<<"20000 time:"<<mt->elapsed()/1000.0<<"s";
```

![image-20220507142453813](C:/Users/boxnc/AppData/Roaming/Typora/typora-user-images/image-20220507142453813.png)

数据库table->params->ModelVersions下，增加储存mutilset的字段，储存图片+label的hash值



获得模型版本

```c++
void AQLocationMatch::get_current_model_time(const OperatorInfo &info, std::string &model_vision, QString &model_time, long &train_time)
{
    QString table_name = "params";
    QString operator_name = info.dir_name + "_params";
    QByteArray array = aq::APSQLManager::database_manager(info.project_db_connectname)->get_model_versions(table_name, operator_name);
    QJsonParseError jsonError;
    QJsonDocument doucment = QJsonDocument::fromJson(array, &jsonError);
    if (!doucment.isNull() && (jsonError.error == QJsonParseError::NoError)){
        if (doucment.isObject()){
            QJsonObject params = doucment.object();
            int version = params.value("Version").toInt();

            model_vision = "V" + std::to_string(version);
            model_time = params.value("ModelVersions").toObject().value(QString::number(version)).toObject().value("Test").toObject().value("ModelTime").toString();
            train_time = params.value("ModelVersions").toObject().value(QString::number(version)).toObject().value("Test").toObject().value("TrainTime").toInt();
        }
    }
}

```



版本转换

```c++
void AQIncrementalThread::slt_convert_project(bool flag, ProjectInfo info, bool cloud, bool convert)
```



incremental_hash字段生成

```c++
QJsonObject AQSqlOperator::records_params(aq::TestRecord test_record, aq::TestMatrixRecord matrix_record)
```

接下来

```
void AIOperatorModule::slt_train_thread()
"ModelVersions"
```

是否有此表

```c++
if(aq::APSQLManager::database_manager(project_db_connectname_)->is_exists_operator_name(table_name_, operator_name_)){
        return ;
```

emit

```c++
emit sig_update_record(record, OperatorEnum::MODE_CLASSIFY);
```

update

```c++
aq::APSQLManager::database_manager(project_db_connectname_)->update_model_version(table_name_,operator_name_, model_version);
```

**to db：先把modelversions字段拿出来，**

```c++
QByteArray SQLQueryOperator::get_model_versions(const QString &table_name, const QString &name)
```

**然后写入hash字段后，update**

```c++
aq::APSQLManager::database_manager(project_db_connectname_)->update_model_version(table_name_,operator_name_, model_version);
```

**vector to json**

```c++
QJsonObject AQSqlOperator::records_params(aq::TestRecord test_record, aq::TestMatrixRecord matrix_record,QVector<std::string> increment_hash_record)
```

**训练结束**

```c++
//get and save image hash
```

点击训练，处理hash判断逻辑

```c++
void AQToolBar::slt_module_train(OperatorInfo info)
```

**最终在训练逻辑中的保存hash函数**

```c++
void AIOperatorModule::save_increment_hash()
```

**hash计算时，模态**

```c++
void AQWaitingBar::slt_receive_event(AQProtocol protocol)
```

```c++
void AQAIDIProjectModule::train(QString model_version) // 获取训练集的位置
```



```c++
void AIOperatorModule::slt_auto_select_thread() //自动挑选样本算法的位置
```

swap json

```c++
//#pragma region "swap json" {
//                QString v_path = model_path + AQ_FILE_TRAIN_JSON;
//                QByteArray v_json ;
//                QJsonObject v_model_params;
//                QJsonArray img_mean;
//                AQLocationMatch::get_file(v_path,v_json);
//                if(AQLocationMatch::bytearray_to_json(v_json,v_model_params)){
//                    img_mean = v_model_params.value("g_common").toObject().value("value").toObject().value("img_mean").toObject().value("value").toArray();
//                }

//                QString model_path = info.project_dir + "/" + info.dir_name + "/model" + AQ_FILE_TRAIN_JSON;
//                QByteArray model_json;
//                QJsonObject model_params;
//                int batch_size;
//                int epoch;
//                int gpu_num;
//                AQLocationMatch::get_file(model_path,model_json);
//                if(AQLocationMatch::bytearray_to_json(model_json,model_params)){
//                    batch_size = model_params.value("g_common").toObject().value("value").toObject().value("batch_size").toObject().value("value").toInt();
//                    epoch = model_params.value("g_common").toObject().value("value").toObject().value("epoch").toObject().value("value").toInt();
//                    gpu_num = model_params.value("g_common").toObject().value("value").toObject().value("gpu_num").toObject().value("value").toInt();

//                    QJsonValueRef g_common_ref = model_params.find("g_common").value();
//                    QJsonObject g_common_obj = g_common_ref.toObject();
//                    QJsonObject g_common_value_obj = g_common_obj.value("value").toObject();
//                    QJsonObject img_mean_obj = g_common_value_obj.value("img_mean").toObject();
//                    img_mean_obj["value"] = img_mean;
//                    g_common_value_obj["img_mean"] = img_mean_obj;
//                    g_common_obj["value"] = g_common_value_obj;
//                    g_common_ref = g_common_obj;
//                }

//                QJsonValueRef g_common_ref = v_model_params.find("g_common").value();
//                QJsonObject g_common_obj = g_common_ref.toObject();
//                QJsonObject g_common_value_obj = g_common_obj.value("value").toObject();
//                QJsonObject batch_size_obj = g_common_value_obj.value("batch_size").toObject();
//                QJsonObject epoch_obj = g_common_value_obj.value("epoch").toObject();
//                QJsonObject gpu_num_obj = g_common_value_obj.value("gpu_num").toObject();
//                batch_size_obj["value"] = batch_size;
//                epoch_obj["value"] = epoch;
//                gpu_num_obj["value"] = gpu_num;
//                g_common_value_obj["batch_size"] = batch_size_obj;
//                g_common_value_obj["epoch"] = epoch_obj;
//                g_common_value_obj["gpu_num"] = gpu_num_obj;
//                g_common_obj["value"] = g_common_value_obj;
//                g_common_ref = g_common_obj;

//                std::string model_hash;
//                QByteArray model_json_byte = QJsonDocument(model_params).toJson();
//                AQLocationMatch::get_json_hash(model_json_byte,model_hash);

//                std::string v_model_hash;
//                QByteArray v_model_json_byte = QJsonDocument(v_model_params).toJson();
//                AQLocationMatch::get_json_hash(v_model_json_byte,v_model_hash);

//                if(v_model_hash == model_hash){

//                }
//                else{
//                    int result = show_information(Q_NULLPTR, tr("prompt"), tr("json diffirent, cover?"),
//                                                  QMessageBox::No | QMessageBox::Yes,
//                                                  QMessageBox::Yes);

//                    if(result == QMessageBox::Yes){
//                        QFile file(model_path);
//                        file.open(QIODevice::ReadWrite|QIODevice::Truncate);
//                        QByteArray data = QJsonDocument(v_model_params).toJson();
//                        file.write(data);
//                        file.close();
////                        emit sig_send_event(protocol);
//                    }
//                    else{

//                    }
//                }
//#pragma endregion }
```

```c++
protocol.insert_cmdvalue(QVariant("busy")); //等待条线程问题
```



hash_dataset_type "dateset_info3"

hash_img_size "img_size16241240"

hash_name "name3"

hash_dataset_type "dateset_info16843009"

hash_img_size "img_size00"

hash_name "name3"

![image-20220618151124861](C:/Users/boxnc/AppData/Roaming/Typora/typora-user-images/image-20220618151124861.png)

![image-20220618151147164](C:/Users/boxnc/AppData/Roaming/Typora/typora-user-images/image-20220618151147164.png)

![image-20220620095103234](C:/Users/boxnc/AppData/Roaming/Typora/typora-user-images/image-20220620095103234.png)
