# MyNote

尝试使用github做笔记

先贴一段代码看看支持性好不好

``` go
	user := UserSig{AppId: IMSdkAppIdInt, 
					Identifier: UserIdentifier, Expire: 7 * 24 * 3600}
	sig := user.GetUserSig()

	if sig == "" {
		return "", ErrGetUserSigFail
	}

	randSeed := rand.New(rand.NewSource(time.Now().UnixNano()))
	rNum := fmt.Sprintf("%v", randSeed.Uint32())

	url := ApiUrl + "group_open_http_svc/create_group?usersig=" +
		sig + "&identifier=" + UserIdentifier + "&sdkappid=" + IMSdkAppId + "&random=" + rNum + "&contenttype=json"

	var chatRoomType string
	if chatType != ChatRoomLiveType {
		chatRoomType = ChatRoomFreeType
	} else {
		chatRoomType = ChatRoomLiveType
	}

	msg := CreateChatRoomInfo{
		Type:            chatRoomType,
		Name:            ChatRoomPrefix + strconv.Itoa(channelId),
		ApplyJoinOption: ApplyJoinOption,
		MaxMemberCount:  MaxMemberCount,
		Owner_Account:   OwnerPrefix + strconv.Itoa(owner),
	}

	msgJson, e := json.Marshal(&msg)
	if e != nil {
		return "", ErrGetJsonUnmarshalFail
	}

	h := make(common.RequestHeader)
	h.Set("Content-Type", "application/json")

	proxy := common.ProxyContext{
		Url:    url,
		Method: http.MethodPost,
		Header: h,
		Body:   bytes.NewBuffer(msgJson),
	}

	_, respBody, e := proxy.Do()
```
