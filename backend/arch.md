```
sidekiq worker call trigger
{
	"channel":"dhcp-139-n102.dhcp.perforce.com-7"},
	"event":"new3",
	"data":"{\"error\":{\"message\":\"You are not allowed to create any more projects.\"}}"
	"params":{\"key1\":\"value1\",\"key2\":\"value2\"}
}

which in turn post to pusher service:
{
	verb=post,
	url=http://api.pusherapp.com:80/apps/81987/events,
	params={}, 
	body="{\"key1\":\"value1\",\"key2\":\"value2\",\"name\":\"new8\",\"channels\":[\"dhcp-139-n102.dhcp.perforce.com-7\"],\"data\":\"{\\\"error\\\":{\\\"message\\\":\\\"You are not allowed to create any more projects.\\\"}}\"}"
}


git pull
git co max_projects_count_20150128_WIP
git rebase master
git co master
git merge max_projects_count_20150128_WIP
```
