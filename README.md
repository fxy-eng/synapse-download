# synapse-download
synapse python下载
![image](https://github.com/user-attachments/assets/8318a1a9-50b6-4a48-9383-8b4086046e1c)


import synapseclient
import os

syn = synapseclient.Synapse()
syn.login(authToken="token", silent=False)
download_dir = r"G:\MA\data"
project_id = "syn60868042"


def download_all_files_from_project(project_id, download_dir):
    print(f"正在下载项目：{project_id} 到 {download_dir}")
    # 遍历项目下的所有实体
    for item in syn.getChildren(project_id):
        entity_id = item['id']
        entity_type = item['type']
        # print("entity_id: " + entity_id)
        # print("entity_type: " + entity_type)
        # print("entity_name: " + item['name'])
        # print("   ")

        if entity_type == 'org.sagebionetworks.repo.model.FileEntity':
            # 如果是文件，则下载
            file_entity = syn.get(entity_id, downloadLocation=download_dir)
            print(f"✅ 下载文件：{file_entity.name} -> {file_entity.path}")
        elif entity_type == 'org.sagebionetworks.repo.model.Folder':
            # 如果是文件夹，递归调用
            folder_path = os.path.join(download_dir, item['name'])
            os.makedirs(folder_path, exist_ok=True)
            download_all_files_from_project(entity_id, folder_path)
            
download_all_files_from_project(project_id, download_dir)
