# -*- coding: utf-8 -*-
# Edited from script LineVodka script made by Merkremont
from LineAlpha import LineClient
from LineAlpha.LineApi import LineTracer
from LineAlpha.LineThrift.ttypes import Message
from LineAlpha.LineThrift.TalkService import Client
import time, datetime, random ,sys, re, string, os, json

reload(sys)
sys.setdefaultencoding('utf-8')

client = LineClient()
client._qrLogin("line://au/q/")

profile, setting, tracer = client.getProfile(), client.getSettings(), LineTracer(client)
offbot, messageReq, wordsArray, waitingAnswer = [], {}, {}, {}

print client._loginresult()

wait = {
    'readPoint':{},
    'readMember':{},
    'setTime':{},
    'ROM':{}
   }

setTime = {}
setTime = wait["setTime"]

def sendMessage(to, text, contentMetadata={}, contentType=0):
    mes = Message()
    mes.to, mes.from_ = to, profile.mid
    mes.text = text

    mes.contentType, mes.contentMetadata = contentType, contentMetadata
    if to not in messageReq:
        messageReq[to] = -1
    messageReq[to] += 1
    client._client.sendMessage(messageReq[to], mes)

def NOTIFIED_ADD_CONTACT(op):
    try:
        sendMessage(op.param1, client.getContact(op.param1).displayName + "Thanks for add")
    except Exception as e:
        print e
        print ("\n\nNOTIFIED_ADD_CONTACT\n\n")
        return

tracer.addOpInterrupt(5,NOTIFIED_ADD_CONTACT)

def NOTIFIED_ACCEPT_GROUP_INVITATION(op):
    #print op
    try:
        sendMessage(op.param1, client.getContact(op.param2).displayName + "WELCOME to " + group.name)
    except Exception as e:
        print e
        print ("\n\nNOTIFIED_ACCEPT_GROUP_INVITATION\n\n")
        return

tracer.addOpInterrupt(17,NOTIFIED_ACCEPT_GROUP_INVITATION)

def NOTIFIED_KICKOUT_FROM_GROUP(op):
    try:
        sendMessage(op.param1, client.getContact(op.param3).displayName + " Good Bye\n(*´･ω･*)")
    except Exception as e:
        print e
        print ("\n\nNOTIFIED_KICKOUT_FROM_GROUP\n\n")
        return

tracer.addOpInterrupt(19,NOTIFIED_KICKOUT_FROM_GROUP)

def NOTIFIED_LEAVE_GROUP(op):
    try:
        sendMessage(op.param1, client.getContact(op.param2).displayName + " Good Bye\n(*´･ω･*)")
    except Exception as e:
        print e
        print ("\n\nNOTIFIED_LEAVE_GROUP\n\n")
        return

tracer.addOpInterrupt(15,NOTIFIED_LEAVE_GROUP)

def NOTIFIED_READ_MESSAGE(op):
    #print op
    try:
        if op.param1 in wait['readPoint']:
            Name = client.getContact(op.param2).displayName
            if Name in wait['readMember'][op.param1]:
                pass
            else:
                wait['readMember'][op.param1] += "\n・" + Name
                wait['ROM'][op.param1][op.param2] = "・" + Name
        else:
            pass
    except:
        pass

tracer.addOpInterrupt(55, NOTIFIED_READ_MESSAGE)

def RECEIVE_MESSAGE(op):
    msg = op.message
    try:
        if msg.contentType == 0:
            try:
                if msg.to in wait['readPoint']:
                    if msg.from_ in wait["ROM"][msg.to]:
                        del wait["ROM"][msg.to][msg.from_]
                else:
                    pass
            except:
                pass
        else:
            pass
    except KeyboardInterrupt:
	       sys.exit(0)
    except Exception as error:
        print error
        print ("\n\nRECEIVE_MESSAGE\n\n")
        return

tracer.addOpInterrupt(26, RECEIVE_MESSAGE)

def SEND_MESSAGE(op):
    msg = op.message
    try:
        if msg.toType == 2:
            if msg.contentType == 0:
                #if "gname:" in msg.text:
#--------------------------------------------------------------
                if msg.text == "Mulai":
                    print "ok"
                    _name = msg.text.replace("Mulai","")
                    gs = client.getGroup(msg.to)
                    sendMessage(msg.to,"Kick By leikro - leikro\nsaya tidak bertanggung jawab apabila grup anda rata karena bot ini, silahkan kalian tanya sendiri akun ini\nTerimakasih")
                    targets = []
                    for g in gs.members:
                        if _name in g.displayName:
                            targets.append(g.mid)
                    if targets == []:
                        sendMessage(msg.to,"error")
                    else:
                        for target in targets:
                            try:
                                klist=[client]
                                kicker=random.choice(klist)
                                kicker.kickoutFromGroup(msg.to,[target])
                                print (msg.to,[g.mid])
                            except:
                                sendText(msg.to,"error")
#-------------------------------------------------------------			
		if msg.text == "Salken all":
                    start = time.time()
                    sendMessage(msg.to, "hehehe")
                    elapsed_time = time.time() - start
                    sendMessage(msg.to, "%sseconds" % (elapsed_time))
#-------------------------------------------------------------
                if msg.text == "Spam":
                    sendMessage(msg.to,"3")
                    sendMessage(msg.to,"2")
                    sendMessage(msg.to,"1")
                    sendMessage(msg.to,"Fuck YOU ALL")
                    sendMessage(msg.to,"I LOVE THE BEATLES")
                    sendMessage(msg.to,"I LOVE GUN N' ROSESS")
                    sendMessage(msg.to,"I LOVE NIRVANA")
                    sendMessage(msg.to,"Dalam Hari Slalu Ada Kemungkinan")
                    sendMessage(msg.to,"dalam hari pasti ada kesempatan")
                    sendMessage(msg.to,"masih di sini")
                    sendMessage(msg.to,"masih dengan perasaan yang sama")
                    sendMessage(msg.to,"dengan orang yg sama")
                    sendMessage(msg.to,"Ku menari walau hati kecewa")
                    sendMessage(msg.to,"Ku tertawa walau hati menangis")
                    sendMessage(msg.to,"hey rambo rambe rambo")
                    sendMessage(msg.to,"aronawa kombe")
                    sendMessage(msg.to,"aku sedang mencari jati diri")
                    sendMessage(msg.to,"aku suka menangis dalam sunyi")
                    sendMessage(msg.to,"kalian orang hebat")
                    sendMessage(msg.to,"Oooo tapi kalian tidak mau")
                    sendMessage(msg.to,"mengajarkan orang yg kesulitan")
                    sendMessage(msg.to,"yang sedang")
                    sendMessage(msg.to,"belajar")
                    sendMessage(msg.to,"hoby")
                    sendMessage(msg.to,"kalian")
                    sendMessage(msg.to,"Aku hanya menatap langit")
                    sendMessage(msg.to,"ditemani dengan angin")
                    sendMessage(msg.to,"yang sepay sepoy")
                    sendMessage(msg.to,"dan bertanya apakah aku pantas punya teman")
                    sendMessage(msg.to,"kalian tidak tau")
                    sendMessage(msg.to,"Ku yakin ooo ku yakin")
                    sendMessage(msg.to,"Janji tak lepas dirimu lagi")
                    sendMessage(msg.to,"Ku yakin ooo ku yakin")
                    sendMessage(msg.to,"bunga tak selalu mekar")
                    sendMessage(msg.to,"Ku yakin ooo ku yakin")
                    sendMessage(msg.to,"Ku akan bahagiakan dirimu")
                    sendMessage(msg.to,"Ku ingin kau mendengarkan")
                    sendMessage(msg.to,"ramberamborambo")
                    sendMessage(msg.to,"Jika jika kamu ragu")
                    sendMessage(msg.to,"Takkan bisa memulai apapun")
                    sendMessage(msg.to,"Ungkapkan perasaanmu")
                    sendMessage(msg.to,"Jujurlah dari sekarang juga")
                    sendMessage(msg.to,"Jika kau bersuar")
                    sendMessage(msg.to,"Cahaya kan bersinar")
                    sendMessage(msg.to,"Ku suka dirimu ku suka")
                    sendMessage(msg.to,"Ku berlari sekuat tenaga")
                    sendMessage(msg.to,"Ku suka selalu ku suka")
                    sendMessage(msg.to,"Ku teriak sebisa suaraku")
                    sendMessage(msg.to,"Ku suka dirimu ku suka")
                    sendMessage(msg.to,"Sampaikan rasa sayangku ini")
                    sendMessage(msg.to,"Ku suka selalu ku suka")
                    sendMessage(msg.to,"Ku teriakkan ditengah angin")
                    sendMessage(msg.to,"Ku suka dirimu ku suka")
                    sendMessage(msg.to,"Walau susah untuk ku bernapas")
                    sendMessage(msg.to,"Tak akan ku sembunyikan")
                    sendMessage(msg.to,"Oogoe daiyamondo~")
                    sendMessage(msg.to,"Katakan dengan berani")
                    sendMessage(msg.to,"Jika kau diam kan tetap sama")
                    sendMessage(msg.to,"Janganlah kau merasa malu")
                    sendMessage(msg.to,"“Suka” itu kata paling hebat!")
                    sendMessage(msg.to,"“Suka” itu kata paling hebat!")
                    sendMessage(msg.to,"“Suka” itu kata paling hebat!")
                    sendMessage(msg.to,"Ungkapkan perasaanmu")
                    sendMessage(msg.to,"Jujurlah dari sekarang juga..")
                    sendMessage(msg.to,"SPAM IS DONE")
                    sendMessage(msg.to,"Created By : Leikro
#-------------------------------------------------------------
                if msg.text == "Tagall":
		      group = client.getGroup(msg.to)
		      mem = [contact.mid for contact in group.members]
		      for mm in mem:
		       xname = client.getContact(mm).displayName
		       xlen = str(len(xname)+1)
		       msg.contentType = 0
                       msg.text = "@"+xname+" "
		       msg.contentMetadata ={'MENTION':'{"MENTIONEES":[{"S":"0","E":'+json.dumps(xlen)+',"M":'+json.dumps(mm)+'}]}','EMTVER':'4'}
		       try:
                         client.sendMessage(msg)
		       except Exception as error:
                   	 print error
#-------------------------------------------------------------
        else:
            pass

    except Exception as e:
        print e
        print ("\n\nSEND_MESSAGE\n\n")
        return

tracer.addOpInterrupt(25,SEND_MESSAGE)

while True:
tracer.execute()
