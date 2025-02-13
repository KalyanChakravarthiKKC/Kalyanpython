<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Browser Isolation &middot; Zscaler</title>
<style type="text/css">iframe {
border: 0;
bottom: 0;
height: 100%;
left: 0;
position: fixed;
right: 0;
top: 0;
width: 100%;
}
</style>
</head>
<body>
<script type="text/javascript">var ajax = function ajax(url, callback, data, xhr) {
xhr = new XMLHttpRequest();
xhr.open(data ? 'POST' : 'GET', url, 1);
xhr.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
xhr.withCredentials = true;
xhr.onreadystatechange = function () {
xhr.readyState > 3 && callback && callback(xhr.responseText, xhr, url);
};
xhr.send(data);
};
var getErrorUrl = function getErrorUrl(url, status, code) {
return !url
? false
: url.substr(0, url.lastIndexOf('/')) + '/zia-session/logout?status=' + (status || 'error') + '&xCode=' + (code || 'x620X');
};
var renderCb = function renderCb(res, xhr, url, data, redirectUrl) {
try {
if (xhr.status >= 200 && xhr.status < 400) {
data = JSON.parse(res);
if (data.url && data.jwt) {
var html =
'<form method="post" action="' + encodeURI(data.url) + '" id="CBIRequestForm">' +
'<input type="hidden" name="jwt" value="' + encodeURI(data.jwt) + '">' +
'<input id="CBISubmitButton" type="submit" value="Submit">' +
'<\/form>';
var script = document.createElement('script');
script.text =
'document.getElementById(\'CBISubmitButton\').style.visibility = \'hidden\';' +
'document.getElementById(\'CBIRequestForm\').submit();';
if (data.redirect) {
document.body.innerHTML = html;
document.body.appendChild(script);
} else {
var iframe = document.createElement('iframe');
iframe.src = 'data:text/html;charset=utf-8,' + encodeURI(html + script.outerHTML);
document.body.appendChild(iframe);
var refocus = function refocus() {
iframe.focus();
};
document.addEventListener('click', refocus);
document.addEventListener('keydown', refocus);
}
} else {
throw new Error('Incomplete JSON data');
}
} else {
data = JSON.parse(res);
if (data.message) {
// "zsorg fetch failed for profile"
if (data.message === 'zsorg fetch failed for profile') {
window.location.href = url;
}
// "ECTX form was malformed"
// "ECTX could not be decrypted"
// "Internal context could not be encrypted"
// "Inner context URL decode failure"
// "Couldn't encode userAuthToken token"
// "Error marshalling response as json"
else {
throw new Error(data.message);
}
} else {
throw new Error('Unknown JSON data');
}
}
} catch (err) {
console.error('Response status: ' + xhr.status, err.message);
// Invalid JSON or HTML response
let resData;
try {
resData = JSON.parse(xhr.responseText);
} catch (e) {
console.error(e);
}
window.location.href = getErrorUrl(url, 'error', resData.xCode ? resData.xCode : 'x620X');
}
};
var isZgpuSupported = function isZgpuSupported() {
return !!document.createElement('canvas').getContext('webgl2', {
failIfMajorPerformanceCaveat: true,
});
}
var init = function init(url, ectx) {
var rsrc = 'main';
if (window.self !== window.top) {
rsrc = 'iframe';
};
ajax(url, renderCb, JSON.stringify({
ectx: ectx,
source: rsrc,
nozgpu: !isZgpuSupported(),
}));
};
</script>
<script type="text/javascript">
var url = 'https://redirect.isolation.zscaler.com/tenant/462f064b6b51/profile/1233fc6e-6618-4022-a03b-96afce7da312/sessions';
var ectx = 'JeB5aBuVFfwCdCxhz4K+vPwV/z+lsp2QQzzfmwPN8yfklMwow78Mw6J+7M4Ef6KCh5sgiA1ppCGIeUK3B6uflyaqtNCsctoTxb+qClyQq8UFurVJ6Cwvi2LUqo2/YQQscUb9ELRQy1uk0oKRPeCbL3rWEKyGnAPQ0+yveRCKmdCWCsvCneunFQuuA2460s4a+q6asGTxjT/Ajf+vP56ZS6HzANvHlQVb21XvxS8ltZlu2jPmt/AIRO8sATYOtCLz+fw28p0aiteWSL/OwbwAYMiXKx7n4tH6mGISOk1lVSEecm4CYgGz3EtuhvljR1sencly79IV+37wnf7rimXv4o11b33tBXj1TwYtJEHulRMd7aCunK3zEaxM1zEhZ/ZphIGN+p+3p+vHMkSvm4gEL1ObIfTTJ7/VYdRSluXB7JzHRh9psDPX2sUK7H9uH+Jm9TssOqSQyt5lGqkQF4ubUl7N4533mBuEJtWJkzajic1PRyACbOZSIPWJoioteJ2tbU9b/ZlBfGCdfjdTgSbbNm0Dtrz6A0YZTV2STkUA7IgBcX3FT8Im45dKqDhKpckazgG1G77JFh08ctNW9ZGqOkrurFR4EBLjcQA/hG4zQiWcppJPsGGAjHA9RqLS/76xPZTdWOONka49GDWeYLdMrZ4ikdb6b/vPMQr1rSMsOgrJEOQw6bPS9d3W+qOW5Sz0AGIAz+VWFE8Mjg1RG0Uar+PUQCz5nyxiwIFdmR3j5mxlgRht9SNcqz78NnFb5SSgvzddWijEC3r/u7XBA9HgcqJ4jrs+xaEDKHk2/lBFB7NxkAmdQhmL4C97kXKNOIp8ZcJLPnA2JOU9cM3ILWBSOr8zyvdQ34zcDvTwattHKuWlZpvW2JeapuiMxIumaRXKLT97C7TTa6+jt4dvJlSV+guUWPsL9tEkMY2Uc60ZyCBMPD3bHQXLw9ZSKAmXI7Y3YvSiYBEgScBBlyoUqMOg+RjcIAYFxJJrFr3Bzj6/Rjjy+nIi1BfsAZEdim3oel1gFljLF5WOxrJsbcoD+FI21cMUSid4QdcEAy2MBfKgST4GnaKff4O9Re1Z2IQp4s5sLOrBahSdEbKlmm033NBWa9h7SNbZDS2bnzzMNQJf/qxzxBZ1RSlkY8YPNJhbeu2kOLfJXT2nWM7TU2vGjsaQUY5pw145Vpi/7y46jNtBMeH37vlRJJWEquytwBg1MCg3njoF8foT2iQsied5ckfDhUmAwmZXTLqUagShSXrkMcQjyv480Klmkv1/M3yJ/oUt75707LZ+C2JNuydhg6a+0nLETMzcFnRFjqp/xIUXB8LBd2R3sF9VZvMECy==';
init(url, ectx);
</script>
<noscript>JavaScript is disabled or unsupported. Please enable JavaScript and try reloading the page.</noscript>
</body>
</html>
<!-- 748112 3 190 0 1739172533 192 https://github.com/Tanu-N-Prabhu/Python/raw/refs/heads/master/Python%20Coding%20Interview%20Prep/Python%20Coding%20Interview%20Questions%20(Beginner%20to%20Advanced).md -->