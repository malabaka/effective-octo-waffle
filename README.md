写一个web监控的代码，每隔一段时间去访问一个网站，如果网站返回的状态码不是200，就发送一个报警邮件。

# 创建一个文件夹
mkdir -p /home/python/web_monitor

# 创建一个脚本文件
vim /home/python/web_monitor/web_monitor.py

# 写入以下代码
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import requests
import smtplib
from email.mime.text import MIMEText
from email.header import Header

# 发送邮件的邮箱地址
from_addr = 'xxx@163.com'
# 发送邮件的邮箱密码
password = 'xxx'
# 接收邮件的邮箱地址
to_addr = 'xxx@qq.com'
# 发送邮件的SMTP服务器地址
smtp_server = 'smtp.163.com'
# 监控的网站地址
url = 'http://www.baidu.com'

def send_mail(content):
    msg = MIMEText(content, 'plain', 'utf-8')
    msg['From'] = Header(from_addr)
    msg['To'] = Header(to_addr)
    msg['Subject'] = Header('网站监控')

    server = smtplib.SMTP(smtp_server, 25)
    server.login(from_addr, password)
    server.sendmail(from_addr, [to_addr], msg.as_string())
    server.quit()

def monitor():
    try:
        r = requests.get(url)
        if r.status_code != 200:
            content = '网站异常，状态码为：%s' % r.status_code
            send_mail(content)
    except Exception as e:
        content = '网站异常，异常信息为：%s' % e
        send_mail(content)

if __name__ == '__main__':
    monitor()

# 保存退出

# 修改脚本文件
