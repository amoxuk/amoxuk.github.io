# 剪映视频播放解码

视频源地址：<https://learning.capcut.cn/topic_detail/7128272592783428894/10?page_enter_from=videocut_pc>


从`https://vod.bytedanceapi.com/?Action=GetPlayInfo`接口获取下载地址

`https://v3-jianying.vlabvod.com/177fe74c6ab9807d9e12cafbd7af1070/6366871a/video/tos/cn/tos-cn-ve-0000/8619dc0dedae44dd9cc4917c5a352b71/?a=1775&ch=0&cr=0&dr=0&er=1&lr=jianying&cd=0%7C0%7C0%7C0&cv=1&br=523&bt=523&cs=0&ds=3&ft=aX_4EQQqUYqfJEZao0OW_EklpPiXEBZwOVJECI53rbPD-Ani&mime_type=video_mp4&qs=0&rc=ODloN2Y0aTg8OjplODZlOEBpMzxpOWY6ZnFwZTMzNDU7M0BiYy4tMzRgNl4xYC0tNV9iYSM0NGZtcjQwM2pgLS1kYC9zcw%3D%3D&l=20221105224936010212172039058ACE1B`


从`https://learning.capcut.cn/video/drm/v1/play_licenses`接口获取加密的key

`zOYYqFeKGqsGtkipVrNK/1WvHrRUr0q3BpdMjVbDTdwBwRvo6A==`

解密后为
`cdf8d843e6614d8fb2b260e7b23f48f3`

使用命令行解密:
`ffmpeg -decryption_key cdf8d843e6614d8fb2b260e7b23f48f3 -i index.mp4 -c copy -copyts -movflags +faststart -movflags +use_metadata_tags -f mp4 -y decrypt.mp4`
