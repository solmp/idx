<!-- # Window cmd命令
-  -->

## Anaconda
- 官网地址：https://www.anaconda.com/
- 官方参考文档
    - 英文版：https://docs.anaconda.com/
    - 中文版：https://anaconda.org.cn/

### 清理anaconda
- 删除索引缓存、锁定文件、未使用过的包和tar包: `conda clean -a`

### 更新anaconda
- `conda update conda` 
- `conda install anaconda=<版本号>`
    - 例：`conda install anaconda=2022.5`
- 更新当前环境所有包: `conda update --all`
    - 指定环境：`conda update -n xxx --all`


### 环境管理
- conda 导出内容包括 pip 中内容
- 查看所有环境: `conda env list`
- 切换环境: `conda activate xxx`
- 导出环境
    - conda: `conda env export > xxx.yaml`
    - pip: `pip freeze > xxx.txt`
- 导入环境
    - conda: `conda env create -f xxx.yaml`
    - pip: `pip install -r xxx.txt`

## jupyter内核
虚拟环境内核配置存放路径：`C:\Users\solen\AppData\Roaming\jupyter\kernels`
~~~json
# C:\Users\solen\AppData\Roaming\jupyter\kernels\<内核名>\kernel.json
{
 "argv": [
  "D:\\ProgramData\\Anaconda3\\envs\\<内核名>\\python.exe",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "<内核名>",
 "language": "python",
 "metadata": {
  "debugger": true
 }
}
~~~
