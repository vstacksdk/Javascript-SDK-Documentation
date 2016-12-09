# 1. Quick Start:

Download JavaScript SDK and example at: http://developer-vstack.vht.com.vn/SDK/javascript

Please review example (authentication, chat):


```javascript
<!-- optional -->
<script type="text/javascript" src="js/jquery-1.12.1.min.js"></script>

<!-- required libraries -->
<script type="text/javascript" src="js/socket.io.js"></script>
<script type="text/javascript" src="js/jsencrypt.js"></script>
<script type="text/javascript" src="js/adapter-1.3.0.js"></script>
<script type="text/javascript" src="js/vstack-sdk-1.5-build-20161118.js"></script>

<script type="text/javascript">
	var currentMsgId = Math.floor(Date.now() / 1000);

	$(document).ready(function () {
		//init SDK
		var azAppID = "8f6b6d607b7b6f5ec45b367c1c97ca68"; // you will get it after registered
        var publicKey = 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEApxDkgYRgUr24soveQzHXE9GqQLi8Kv0KJtd35glW8UHry6Vy7o2fCGMNDTewKLrQmFLvAoMavIT+ZWKGG0yaARJQ93GduKmQ1C9UAgc3hLlHfW/YabwgENCkUdKtrnLZdx603wxxCfrmehP7LsqEp8BaHQJeTy4FLdCofTqNb8sR836Vk5CWoc11RoYsFJqV6htmxTgpxaHG3jJZOj15ran2rDSN7/yQc+nl8YIEeqmppYWupAe1/N3MkXwmTUPqgQUkXwPbYbuvHtLKLtOaHqSjQ88Vppjg0igZO6gTjL0vY8eumtePb61Ylv8JXzZ6Y+CXoE6x8/15rjG8jCMFvwIDAQAB';
        var VStackUserId = 'test';
        var fullname = 'Tester';
        var userCredentials = '';
        var namespace = '';
		//log level
		vstack.logLevel = 'INFO';

		//SDK delegate --------------------------------------------- -->
		vstack.onAuthenticationCompleted = function (code, authenticatedUser, msg) {
			vstack.log('INFO', 'onAuthenticationCompleted code: ' + code + ', msg: ' + msg + ', authenticatedUser');
			vstack.log('INFO', authenticatedUser);

			$msgDiv.append('onAuthenticationCompleted code ' + code + ', authenticatedUser: ' + '<br />');
			$msgDiv.append(JSON.stringify(authenticatedUser) + '<br />');
		}
		vstack.onMessageReceived = function (user, msg) {
			vstack.log('INFO', 'onMessageReceived');
			vstack.log('INFO', user);
			vstack.log('INFO', msg);
		}
		vstack.onMessagesDelivered = function (packet) {
			vstack.log('INFO', 'onMessagesDelivered');
			vstack.log('INFO', packet);
		}
		vstack.onMessagesSent = function (packet) {
			vstack.log('INFO', 'onMessagesSent');
			vstack.log('INFO', packet);
		}
		vstack.onMessageFromMe = function (packet) {//msg from me (from other device)
			vstack.log('INFO', 'onMessageFromMe');
			vstack.log('INFO', packet);
		}
		//SDK delegate --------------------------------------------- <--

		//start authentication
		vstack.connect(azAppID, publicKey, VStackUserId, userCredentials, fullname, namespace);//connect VStack server
	});
</script>
```

# 2. Create and configuration your application:
>	a. Create your application (project) here: http://developer-vstack.vht.com.vn/project/add

>	b. After you created successfully, click on menu "Keys", press "Generate a new keys" button to get your Public key, Private key and Secret code. Private key and Secret code will be stored in in your server to do authentication. The 3-ways authentication model between Client (web app) - VStack server - your server, as described below: 
https://github.com/vstacksdk/android-sdk-documentation#33-connect-and-authenticate

>	c. Please refer to example of server authentication is here: https://github.com/vstacksdk/backend-example/tree/master/php
	Authentication URL is configured at menu "Authen URL" in http://developer-vstack.vht.com.vn

# 3. Using JavaScript SDK:
Required libraries:

> a. socket.io: https://github.com/socketio/socket.io-client

> b. jsencrypt: https://github.com/travist/jsencrypt

> c. VStack SDK

Optional libraries:
>  JQuery: https://jquery.com/download/


Sample code:
```javascript
		<script type="text/javascript" src="js/socket.io.js"></script>
                <script type="text/javascript" src="js/jsencrypt.js"></script>
		<script type="text/javascript" src="js/adapter-1.3.0.js"></script>
                <script type="text/javascript" src="js/vstack-sdk-1.5-build-20161811.js"></script>
```
# 4. Delegates

Delegate functions is required to receive successful events (receive incoming message, send message):
```javascript
//Call this function after authentication process is completed
vstack.onAuthenticationCompleted = function (code, authenticatedUser, msg) {
}

//Function triggered when receiving incoming message
vstack.onMessageReceived = function (user, msg) {
}

//Function triggered when receiver receive message
vstack.onMessagesDelivered = function (packet) {
}

//Function triggered when send message successfully
vstack.onMessagesSent = function (packet) {
}

//Function triggered when receive message in other devices
vstack.onMessageFromMe = function (packet) {
}

//Triggered when receive message from group
vstack.onGroupMessageReceived = function (msg) {
}

//Triggered when retrieving list conversation
vstack.onListModifiedConversationReceived = function (packet) {
}

//Triggered when retrieving list of read messages
vstack.onListModifiedMessagesReceived = function (packet) {
}

//Triggered when retrieving list of Unread messages
vstack.onListUnreadMessagesReceived = function (packet) {
}

//Triggered when you are a member in a newly created group (someone created group that have you as member)
vstack.onMakeGroupNotification = function (packet) {
}

//Triggered when another member is typing in this group
vstack.chat_group_typing_processor = function (service, body) {
}

//Triggered when someone is typing with current user
vstack.chat_typing_processor = function (service, body) {
}

//Triggered when someone invited another member in the group that you are currently a member
vstack.onInviteGroupNotification = function (packet) {
}

//Triggered when someone quit group
vstack.onLeaveGroupNotification = function (packet) {
}

//Triggered when group name is edited
vstack.onRenameGroupNotification = function (packet) {
}

//Triggered when conversation is deleted
vstack.onDeleteConversation = function (packet) {
}

//Triggered when message is seen
vstack.onSeenMessage = function (packet) {
}

//Triggered when all messages are seen
vstack.onSeenMessages = function (packet) {
}

//Triggered when list of message is deleted
vstack.onDeleteMessage = function (packet) {
}

//Triggered when get list of chat group
vstack.onListChatGroup = function (packet) {
}

//Triggered when group admin has been assigned to another member
vstack.onChatGroupChangeAdmin = function (packet) {
}

//Triggered when receiving list of files
vstack.onListFilesReceived = function (packet) {
}

//Triggered when user status is changed (background or foreground)
vstack.onApplicationChangeState = function (packet) {
}

//Triggered when broadcast is created
vstack.onBroadCastCreateList = function (packet) {
}

//Triggered when broadcast is edited
vstack.onBroadCastEditList = function (packet) {
}

//Triggered when receiving list of broadcast
vstack.onBroadCastGetList = function (packet) {
}

//Triggered when broadcast is sent
vstack.onMessageBroadCast = function(packet){
}

//Triggered when there is new message in broadcast
vstack.onHaveMessageBroadCast = function(packet){
}

//Triggered when broadcast is deleted
vstack.onChatBroadCastDeleteList = function(packet){
}

//Triggered when getting message from broadcast
vstack.onChatBroadCastGetMessages = function(packet){
}

//Triggered when joining channel
vstack.onChatGroupJoinPublic = function(packet){
}

//Triggered when receiving list of channel
vstack.onChatGroupGetListPublic = function(packet){
}

//Triggered when co cuoc goi video/voice
vstack.onInviteVideoCall = function(packet){
}

//Triggered when cuoc goi video/voice stop
vstack.onVideoCallStop = function(packet){
}

//Triggered when cuoc goi video/voice dang bat dau
vstack.onVideoCallConnecting = function(packet){
}

//Triggered when cuoc goi video/voice dang đổ chuông
vstack.onVideoCallRinging = function(packet){
}

//Triggered when cuoc goi video/voice bị từ chối
vstack.onVideoCallRejected = function(packet){
}

//Triggered when cuoc goi video/voice đã trả lời
vstack.onVideoCallAnswered = function(packet){
}

//Triggered when cuoc goi video/voice bận
vstack.onVideoCallBusy = function(packet){
}

//Triggered when cuoc goi video/voice không trả lời
vstack.onVideoCallNotAnswered = function(packet){
}

//Triggered when cuoc goi video/voice lỗi
vstack.onVideoCallError = function(packet){
}

//Triggered when cuoc goi video/voice load được video máy của mình
vstack.onVideoCallLocalVideoLoaded = function(){
}

//Triggered when cuoc goi video/voice load được video máy của người gọi mình
vstack.onVideoCallRemoteVideoLoaded = function(){
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình bân
vstack.onVideoCallBusyByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình trả lời
vstack.onVideoCallAnsweredByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình từ chối
vstack.onVideoCallRejectedByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình stop
vstack.onVideoCallStopByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình không trả lời
vstack.onVideoCallNotAnsweredByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình lỗi
vstack.onVideoCallErrorByMe = function(packet) {
}

```
# 5. connect:
Call this function to connect to VStack server and implement authentication process:
```javascript
vstack.connect(azAppID, publicKey, VStackUserId, userCredentials, fullname);
```
Parameters:

> azAppID: Your application ID, get this value from menu "Keys" at http://developer-vstack.vht.com.vn/

> publicKey: RSA public key of your application, get this from menu "Keys" at http://developer-vstack.vht.com.vn/

> VStackUserId: Unique ID for each user (string) in each application in VStack, which identify unique user in that application. ID can be mobile number, email, username, ...

> userCredentials: can be password or token, ... This information will be forwarded by VStack to your server in authentication process.

> fullname: Name (is used to send push notification to mobile when user is not online)

# 6. azGetListModifiedConversations:
Lấy danh sách conversation sau khi authen thành công:
```javascript
vstack.azGetListModifiedConversations(lastUpdate);
```
Parameters:

> lastUpdate: only get list of conversations that have change after [lastUpdate] (tính bằng miliseconds)

# 7. azGetListModifiedMessages:
Get list of message from 1 conversation:
```javascript
vstack.azGetListModifiedMessages(lastUpdate, type, chatId);
```
Parameters:

> lastUpdate: only get list of conversations that have change after [lastUpdate] (tính bằng miliseconds)

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 8. azGetListUnreadMessages:
Get list of unread message from 1 conversation:
```javascript
vstack.azGetListUnreadMessages(type, chatId);
```
Parameters:

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 9. getUserInfoAndRequestToServer:
Get information from user by userId
```javascript
vstack.getUserInfoAndRequestToServer(userId);
```
Parameters:

> chatId: userId of user

# 10. getUserInfoAndRequestToServerWithCallBack:
Get information of user by userId with callback
```javascript
vstack.getUserInfoAndRequestToServerWithCallBack(userId, callback);
```
Parameters:

> userId: userId of user

> callback: after request to server and get user data, function will be called

# 11. getUserInfoByUsernameAndRequestToServer:
Get information of user by VStackUserID
```javascript
vstack.getUserInfoByUsernameAndRequestToServer(VStackUserID);
```
Parameters:

> VStackUserID: VStackUserID of user

# 12. getUserInfoByUsernameAndRequestToServerWithCallBack:
Get information of user by VStackUserID with callback
```javascript
vstack.getUserInfoByUsernameAndRequestToServerWithCallBack(VStackUserID, callback);
```
Parameters:

> VStackUserID: VStackUserID of user

> callback: after request to server and get user data, function will be called

# 13. azSendMessage:
Send 1 message text chat 1 - 1
```javascript
vstack.azSendMessage(msgContent, VStackUserID, msgId, callback);
```
Parameters:

> msgContent: content of message

> VStackUserID: VStackUserID of user who receive message

> msgId: Id of message must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called

# 14. azSendMessageFileUrl:
Send 1 message picture/audio/video/file chat 1 - 1
```javascript
vstack.azSendMessageFileUrl(url, fileName, msgType, VStackUserID, msgId, fileLength, width, height, duration, callback);
```
Parameters:

> url: file's path

> fileName: file's name

> msgType: msgType = 1 (Photo), msgType = 2 (Voice), msgType = 8 (File), msgType = 9 (Video)

> VStackUserID: VStackUserID of user who receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> fileLength: file size (kilobytes)

> width: width of picture (photo message)

> height: height of picture (photo message)

> duration: thời gian của message (voice message)

> callback: after request to server and get data, function will be called

# 15. azSendMessageSticker:
Send message sticker chat 1 - 1
```javascript
vstack.azSendMessageSticker(VStackUserID, imgName, catId, msgId, url, width, height, callback);
```
Parameters:

> VStackUserID: VStackUserID of user who receive chat message

> imgName: name of sticker

> catId: category of sticker

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of sticker

> width: width of sticker

> height: height of sticker

> callback: after request to server and get data, function will be called

# 16. azSendMessageGroup:
Send one message text chat group
```javascript
vstack.azSendMessageGroup(msgContent, groupId, msgId, callback);
```
Parameters:

> msgContent: content of message

> groupId: groupId of group that receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called

# 17. azSendMessageFileUrlGroup:
Send one message ảnh/audio/video/file chat group
```javascript
vstack.azSendMessageFileUrlGroup(groupId, msgId, url, msgType, fileName, fileLength, width, height, duration, callback);
```
Parameters:

> groupId: groupId of group that receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of file

> msgType: msgType = 1 (Photo), msgType = 2 (Voice), msgType = 8 (File), msgType = 9 (Video)

> fileName: file name

> fileLength: File size of file, (kilobytes)

> width: width of picture, (photo message)

> height: height of picture, (photo message)

> duration: thời gian của message (voice message)

> callback: after request to server and get data, function will be called

# 18. azSendMessageStickerGroup:
Send one message sticker chat group
```javascript
vstack.azSendMessageStickerGroup(groupId, imgName, catId, msgId, url, width, height, callback);
```
Parameters:

> groupId: groupId of group that receive message

> imgName: name sticker

> catId: category of sticker

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of sticcker

> width: width of sticker

> height: height of sticker

> callback: after request to server and get data, function will be called

# 19. azSendMessageReport:
Report that message has receive chat message 1 - 1
```javascript
vstack.azSendMessageReport(toUserId, msgId);
```
Parameters:

> toUserId: userId of sender

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 20. azSendMessageGroupReport:
Report message has receive chat group
```javascript
vstack.azSendMessageGroupReport(group, msgId, msgSender);
```
Parameters:

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> msgSender: userId of sender

# 21. azSendTyping:
Send typing status to 1 conversation
```javascript
vstack.azSendTyping(type, chatId);
```
Parameters:

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 22. azMakeGroup:
Tạo 1 group
```javascript
vstack.azMakeGroup(members, name, msgId, type, callback);
```
Parameters:

> members: array userId of user is member in group

> name: name of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> type: type = 0 (group private), type = 1 (group public / channel)

> callback: after created group, this function will be triggered

# 23. azInviteToGroup:
Add members into group
```javascript
vstack.azInviteToGroup(members, group, msgId, callback);
```
Parameters:

> members: array of userId of user wanted to add into group

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after add new members, this function will be triggered

# 24. azLeaveGroup:
Leave group/kick member of group
```javascript
vstack.azLeaveGroup(leaveUser, group, msgId, callback);
```
Parameters:

> leaveUser: userId of user wanted to leave group/kick member

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after leave group/kick member, this function will be triggered

# 25. azRenameGroup:
Đổi name group
```javascript
vstack.azRenameGroup(name, group, msgId, callback);
```
Parameters:

> name: name group wanted to change

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after change name group, this function will be triggered

# 26. azGetGroupInfo:
Get group information from server
```javascript
vstack.azGetGroupInfo(groupId, callback);
```
Parameters:

> groupId: Id of group

> callback: after receive group information, this function will be triggered

# 27. azSeenMessage:
See 1 message
```javascript
vstack.azSeenMessage(msgId, senderId, type, chatId);
```
Parameters:

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> senderId: userId of user saw message

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 28. azSeenMessages:
See list of message
```javascript
vstack.azSeenMessages(chatId, type, lastMsgCreated, list);
```
Parameters:

> chatId: userId of user or Id of group

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> lastMsgCreated: Time of latest seen message

> list: list of message, example: [{msgId: msgId, sender: senderId}, ...]

# 29. disconnect:
Disconnect with vstack
```javascript
vstack.disconnect();
```
Parameters:

# 30. azDeleteMessage:
Delete 1 message
```javascript
vstack.azDeleteMessage(msgId, senderId, chatId, type, callback);
```
Parameters:

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> senderId: userId of user

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

> callback: after delete message, this function will be triggered

# 31. azListChatGroup:
Get list of groups that user is joining
```javascript
vstack.azListChatGroup();
```
Parameters:

# 32. azListFiles:
Get list of files in all conversation
```javascript
vstack.azListFiles(lastUpdate);
```
Parameters:

> lastUpdate: Only get files that have updated after this time (miliseconds)

# 33. azChatGroupChangeAdmin:
Change admin to group
```javascript
vstack.azChatGroupChangeAdmin(group, newAdmin, msgId);
```
Parameters:

> group: Id of group

> newAdmin: userId of user is going to be admin

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 34. azApplicationChangeState:
Change status of user (background or foreground)
```javascript
vstack.azApplicationChangeState(state);
```
Parameters:

> state: state = 1 (background), state = 2 (foreground)

# 35. azChatBroadCastCreateList:
Create broadcast
```javascript
vstack.azChatBroadCastCreateList(members, name);
```
Parameters:

> members: array userId of users to be added in broadcast

> name: name of broadcast

# 36. azChatBroadCastEditList:
Sửa broadcast
```javascript
vstack.azChatBroadCastEditList(removeUsers, name, id, addUsers, msgId);
```
Parameters:

> removeUsers: array userId of users to be deleted from broadcast

> name: name of broadcast that want to change name (keep old name if dont want to change)

> id: Id of broadcast

> addUsers: array userId of users to be added into broadcast

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 37. azChatBroadCastGetList:
Sửa broadcast
```javascript
vstack.azChatBroadCastGetList(lastUpdate);
```
Parameters:

> lastUpdate: Only get broadcast with changes after this point (miliseconds)

# 38. azMessageBroadCast:
Send text message to broadcast
```javascript
vstack.azMessageBroadCast(broadcast, msg, msgId, callback);
```
Parameters:

> broadcast: Id of broadcast

> msg: content of message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after sent message, this function will be triggered

# 39. azMessageStickerBroadCast:
Send sticker message to broadcast
```javascript
vstack.azMessageStickerBroadCast(broadcast, msgId, imgName, catId, url, width, height, callback);
```
Parameters:

> broadcast: Id of broadcast

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> imgName: name sticker

> catId: category of sticker

> url: file path of sticker

> width: width of sticker

> height: height of sticker

> callback: after sent message, this function will be triggered

# 40. azMessageFileBroadCast:
Send one message ảnh/audio/video/file chat broadcast
```javascript
vstack.azMessageFileBroadCast(broadcast, msgId, type, url, fileName, fileLength, width, height, callback);
```
Parameters:

> broadcast: Id of broadcast receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of file

> msgType: msgType = 1 (Photo), msgType = 2 (Voice), msgType = 8 (File), msgType = 9 (Video)

> fileName: file name

> fileLength: File size of file, (kilobytes)

> width: width of picture, (photo message)

> height: height of picture, (photo message)

> callback: after sent message, this function will be triggered

# 41. azChatBroadCastDeleteList:
Delete broadcast
```javascript
vstack.azChatBroadCastDeleteList(broadcastId);
```
Parameters:

> broadcastId: Id of broadcast to be deleted

# 42. azChatBroadCastGetMessages:
Delete broadcast
```javascript
vstack.azChatBroadCastGetMessages(broadcastId, lastUpdate);
```
Parameters:

> broadcastId: Id of broadcast

> lastUpdate: Only get messages with changes after this point (miliseconds)

# 43. azChatGroupJoinPublic:
Join public group/channel
```javascript
vstack.azChatGroupJoinPublic(group, msgId);
```
Parameters:

> group: Id of group/channel to be joined

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 44. azChatGroupGetListPublic:
Get list of public group/channel
```javascript
vstack.azChatGroupGetListPublic();
```
Parameters:

# 45. azGetListModifiedConversationsPage:
Lấy danh sách conversation sau khi authen thành công theo page, option callback:
```javascript
vstack.azGetListModifiedConversationsPage(page, lastCreated, callback);
```
Parameters:

> page: Lấy danh sách theo page

> lastCreated: only get list of conversations that have created after [lastCreated] (tính bằng miliseconds)

> callback: after request to server and get user data, function will be called

# 46. azGetListModifiedMessagesPage:
Get list of message from 1 conversation theo page, option callback:
```javascript
vstack.azGetListModifiedMessagesPage(page, lastCreated, type, chatId, callback);
```
Parameters:

> page: Lấy danh sách theo page

> lastCreated: only get list of messages that have created after [lastCreated] (tính bằng miliseconds)

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

> callback: after request to server and get user data, function will be called

# 47. azGetListUnreadMessagesPage:
Get list of unread message from 1 conversation theo page, option callback:
```javascript
vstack.azGetListUnreadMessagesPage(page, type, chatId, callback);
```
Parameters:

> page: Lấy danh sách theo page

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

> callback: after request to server and get user data, function will be called

# 48. azStartVideoCall:
Start a call to a user with option video or voice call:
```javascript
vstack.azStartVideoCall(toChatId, callId, localVideoId, remoteVideoId, hasVideo);
```
Parameters:

> toChatId: id của user cuộc gọi thực hiện đến

> callId: 1 số độc nhất không lặp lại

> localVideoId: id của thẻ video/audio hiện hình ảnh / tiếng nói của mình

> remoteVideoId: id của thẻ video/audio hiện thị hình ảnh / tiếng nói của mục tiêu cuộc gọi hướng đến

> hasVideo: true/false ứng với đây là cuộc gọi video/audio

# 49. azAcceptVideoCall:
Accept to anwser a call:
```javascript
vstack.azAcceptVideoCall(fromChatId, callId, localVideoId, remoteVideoId, hasVideo);
```
Parameters:

> fromChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

> localVideoId: id của thẻ video/audio hiện hình ảnh / tiếng nói của mình

> remoteVideoId: id của thẻ video/audio hiện thị hình ảnh / tiếng nói của mục tiêu thực hiện cuộc gọi

> hasVideo: true/false ứng với đây là cuộc gọi video/audio, nhận được trong event onInviteVideoCall

# 50. azRejectVideoCall:
Reject to answer a call:
```javascript
vstack.azRejectVideoCall(fromChatId, callId);
```
Parameters:

> fromChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

# 51. azStopVideoCall:
Stop a call:
```javascript
vstack.azStopVideoCall(fromChatId, callId);
```
Parameters:

> fromChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

# 52. azNotAnsweredVideoCall:
Emit a call is not answered:
```javascript
vstack.azNotAnsweredVideoCall(toChatId, callId);
```
Parameters:

> toChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

# 53. toggleVideoState:
toggle local video with optional param state
```javascript
vstack.azWebRTC.toggleVideoState(state);
```
Parameters:

> state: true/false ứng với trạng thái video bật/tắt, nếu không có biến này, thay đổi trạng thái hiện tại của video tắt -> bật, bật -> tắt

# 54. toggleAudioState:
toggle local audio with optional param state
```javascript
vstack.azWebRTC.toggleAudioState(state);
```
Parameters:

> state: true/false ứng với trạng thái audio bật/tắt, nếu không có biến này, thay đổi trạng thái hiện tại của audio tắt -> bật, bật -> tắt

# 55. updateUserInfoWithCallBack:
cập nhật thông tin user theo userId có callback
```javascript
vstack.updateUserInfoWithCallBack(userId, callback);
```
Parameters:

> userId: user Id của user

> callback: after request to server and get user data, function will be called

# 56. updateUserInfoByUsernameWithCallBack:
cập nhật thông tin user theo VStackUserID có callback
```javascript
vstack.updateUserInfoByUsernameWithCallBack(VStackUserID, callback);
```
Parameters:

> VStackUserID: VStackUserID của user

> callback: after request to server and get user data, function will be called

# 57. azGetBroadcastInfo:
Get broadcast information from server
```javascript
vstack.azGetBroadcastInfo(broadcastId, callback);
```
Parameters:

> broadcastId: Id of broadcast

> callback: after receive broadcast information, this function will be triggered

# 58. azLeaveGroupAndChangeAdmin:
Rời nhóm và đổi admin của nhóm
```javascript
vstack.azLeaveGroupAndChangeAdmin(leaveUser, newAdmin, group, msgId, callback);
```
Parameters:

> leaveUser: User id của người rời nhóm

> newAdmin: User id của admin mới muốn chuyển đến

> group: Id của group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: hàm thực hiện sau khi rời nhóm thành công

# 59. azChargingGetBalance:
Lấy thông tin balace của user
```javascript
vstack.azChargingGetBalance(callback);
```
Parameters:

> callback: hàm thực hiện sau có dữ liệu trả về

# 60. getUserInfoByListUsernameAndRequestToServerWithCallBack:
Get information of user by list VStackUserID with callback
```javascript
vstack.getUserInfoByListUsernameAndRequestToServerWithCallBack(listVStackUserID, callback);
```
Parameters:

> listVStackUserID: listVStackUserID of user

> callback: after request to server and get user data, function will be called

# 61. getUserInfoByListUsernameAndRequestToServer:
Get information of user by list VStackUserID
```javascript
vstack.getUserInfoByListUsernameAndRequestToServer(listVStackUserID);
```
Parameters:

> listVStackUserID: listVStackUserID of user

# 62. updateUserInfoByListUsernameWithCallBack:
cập nhật thông tin user theo list VStackUserID có callback
```javascript
vstack.updateUserInfoByListUsernameWithCallBack(listVStackUserID, callback);
```
Parameters:

> listVStackUserID: listVStackUserID của user

> callback: after request to server and get user data, function will be called

# 63. azSendMessageLocation:
Send 1 message location chat 1 - 1
```javascript
vstack.azSendMessageLocation(long, lat, addr, VStackUserID, msgId, callback);
```
Parameters:

> long: địa chỉ long của location

> lat: địa chỉ lat của location

> addr: địa chỉ của location

> VStackUserID: VStackUserID of user who receive message

> msgId: Id of message must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called

# 64. azSendMessageLocationGroup:
Send 1 message location chat group
```javascript
vstack.azSendMessageLocationGroup(groupId, groupName, long, lat, addr, msgId, callback);
```
Parameters:

> groupId: id của group

> groupName: tên của group

> long: địa chỉ long của location

> lat: địa chỉ lat của location

> addr: địa chỉ của location

> msgId: Id of message must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called