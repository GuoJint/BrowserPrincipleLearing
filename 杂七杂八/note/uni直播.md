### 腾讯云直播进行推拉流管理
使用云直播配置或者nigix-rtmp来自己搭建

### 直播推流
目前推流指令均为mac指令，windows另行查询

通过ffmpeg来进行流管理，通过指令：
通过ffmpeg推视频文件:
ffmpeg -re -i 文件地址 -f flv "推流地址可通过腾讯云生成"

使用设备推流：
ffmpeg -f avfoundaction -framerat 30 -video_size 1280x720 -i "设备名或者序号(0:0)" -vcodec libx264 -preset ultrafast -acodec
libmp3lame -ar 44100 -ac 1 -f flv "推流地址"

播放验证:
ffplay "播放地址"