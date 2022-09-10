- git将submodule有关的信息保存在两个地方：
  - gitmodules在仓库中，有版本控制，修改之后会同步到其他仓库，使用submodule相关命令的时候会自动更新
  - git/config在本地，需要手动更新，或者执行git submodule sync将新的配置从.gitmodules拷贝到.git/config
  - git submodule sync会将submodule远程的 url 配置设置到.gitmodules，并且只会影响.git/config已经有 url 的条目
    - 指定–recursive，将会递归更新注册的submodule
      应用场景
      场景1：添加一个submodule
      git submodule add repo_url local_path
      此命令做三件事：克隆工程到本地；创建/修改 .gitmodules标记submodule的具体信息；更新.git/config文件，增加submodule的地址
      场景 2：删除一个submodule
      删除.git/config相关代码
      删除工程目录下的.gitmodules相关代码
      删除缓存的子模块git rm --cached path_to_submodule(路径最后不要加斜线)
      场景 3：更新submodule的url
      删除.git/config相关代码
      删除工程目录下的.gitmodules相关代码
      执行git submodule sync --recursive更新到本地的配置文件
      场景 4：克隆一个有submodule的项目
      git clone repo_url，submodule的代码不会和父项目一起克隆出来
      git submodule update --init [–recursive]可以检出submodule的代码，recursive适用于嵌套submodule的项目
      场景 5：更新 submodule，域远程仓库同步
      问题
      问题 1：git submodule add时报错A git directory for xxx is found locally with remote(s): origin
      删除.git/config相关代码
      删除工程目录下的.gitmodules相关代码
      删除缓存的子模块`git rm --cached path_to_submodule``(路径最后不要加斜线)
      执行git submodule sync --recursive更新到本地的配置文件
      问题 2：git submodule add时报错Pathspec xxx is in submodule
      删除缓存的子模块git rm --cached path_to_submodule(路径最后不要加斜线)