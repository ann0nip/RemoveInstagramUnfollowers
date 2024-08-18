## Remove Instagram Unfollowers
# Two scripts for unfollowing Instagram users who don’t follow you back.


## **WARNING**: The scripts are calling directly to instagram API to be faster.

### Steps:

1. Login into your IG account and open the developer console tool => (Ctrl+Shift+J(Windows) || ⌘+⌥+I (Mac os)).

2. Copy the code and paste into the console:
 ```js
function getCookie(b){let c=`; ${document.cookie}`,a=c.split(`; ${b}=`);if(2===a.length)return a.pop().split(";").shift()}function sleep(a){return new Promise(b=>{setTimeout(b,a)})}function afterUrlGenerator(a){return`https://www.instagram.com/graphql/query/?query_hash=3dec7e2c57367ef3da3d987d89f9dbc8&variables={"id":"${ds_user_id}","include_reel":"true","fetch_mutual":"false","first":"24","after":"${a}"}`}function unfollowUserUrlGenerator(a){return`https://www.instagram.com/web/friendships/${a}/unfollow/`}let followedPeople,csrftoken=getCookie("csrftoken"),ds_user_id=getCookie("ds_user_id"),initialURL=`https://www.instagram.com/graphql/query/?query_hash=3dec7e2c57367ef3da3d987d89f9dbc8&variables={"id":"${ds_user_id}","include_reel":"true","fetch_mutual":"false","first":"24"}`,doNext=!0,filteredList=[],getUnfollowCounter=0,scrollCicle=0;async function startScript(){for(var c,d,e,b,f,g=Math.floor;doNext;){let a;try{a=await fetch(initialURL).then(a=>a.json())}catch(h){continue}followedPeople||(followedPeople=a.data.user.edge_follow.count),doNext=a.data.user.edge_follow.page_info.has_next_page,initialURL=afterUrlGenerator(a.data.user.edge_follow.page_info.end_cursor),getUnfollowCounter+=a.data.user.edge_follow.edges.length,a.data.user.edge_follow.edges.forEach(a=>{a.node.follows_viewer||filteredList.push(a.node)}),console.clear(),console.log(`%c Progress ${getUnfollowCounter}/${followedPeople} (${parseInt(100*(getUnfollowCounter/followedPeople))}%)`,"background: #222; color: #bada55;font-size: 35px;"),console.log("%c This users don't follow you (Still in progress)","background: #222; color: #FC4119;font-size: 13px;"),filteredList.forEach(a=>{console.log(a.username)}),await sleep(g(400*Math.random())+1e3),scrollCicle++,6<scrollCicle&&(scrollCicle=0,console.log("%c Sleeping 10 secs to prevent getting temp blocked","background: #222; color: ##FF0000;font-size: 35px;"),await sleep(1e4))}c=JSON.stringify(filteredList),d="usersNotFollowingBack.json",e="application/json",b=document.createElement("a"),f=new Blob([c],{type:e}),b.href=URL.createObjectURL(f),b.download=d,b.click(),console.log("%c All DONE!","background: #222; color: #bada55;font-size: 25px;")}startScript()
```

3. It will start checking your followers and print it on the console that ones who are not following you back.


4. Once it finishes printing the users, it will create and download a JSON file with all the users who are not following you back.
***The more users you have to check, more time it will take***

5. I developed a [GUI tool](https://43t6lx.csb.app/) where you can upload the JSON file and select what users you want to keep.

6. Refresh the IG website. (to remove all traces of the last script)

7. Open the developer console tool again and create a variable named `listOfUsers` like this:

```js
const listOfUsers = //PASTE HERE THE LIST OF USERS FROM YOUR CLIPBOARD, RESULTS FROM GUI TOOL
```

8. The last step is copy and paste the code below the variable `listOfUsers`:

```js
function getCookie(b){let c=`; ${document.cookie}`,a=c.split(`; ${b}=`);if(2===a.length)return a.pop().split(";").shift()}function sleep(a){return new Promise(b=>{setTimeout(b,a)})}function unfollowUserUrlGenerator(a){return`https://www.instagram.com/web/friendships/${a}/unfollow/`}const csrftoken=getCookie("csrftoken"),startUnfollow=async()=>{let c=Math.floor,a=0,b=0;for(let d of listOfUsers){try{await fetch(unfollowUserUrlGenerator(d.id),{headers:{"content-type":"application/x-www-form-urlencoded","x-csrftoken":csrftoken},method:"POST",mode:"cors",credentials:"include"})}catch(e){console.log(e)}await sleep(c(2e3*Math.random())+4e3),a++,5<= ++b&&(console.log("%c Sleeping 5 minutes to prevent getting temp blocked","background: #222; color: ##FF0000;font-size: 35px;"),b=0,await sleep(3e5)),console.log(`Unfollowed ${a}/${listOfUsers.length}`)}console.log("%c All DONE!","background: #222; color: #bada55;font-size: 25px;")};startUnfollow()
```

***Now, this will start unfollowing the users that you selected.***

**_This script has been tested on Chrome and Safari - It was made in one night probably exists some bugs. Feel free to report or create pull requests_**

## Legal
Disclaimer: This is not affiliated, associated, authorized, endorsed by, or in any way officially connected with Instagram.

Use it at your own risk!.

## Notes

Part of the script was taken from the repo of [@davidarroyo1234](https://github.com/davidarroyo1234/InstagramUnfollowers)
