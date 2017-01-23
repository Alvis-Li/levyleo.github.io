---
layout: post
title: 根据腾讯视频ID获取视频截图

---

```
function getVideoSnap(a) {
	var c, d, e = 1e8;
	for (var g = 4294967296, h = 0, i = 0; i < a.length; i++) {
		var j = a.charCodeAt(i);
		h = 32 * h + h + j, h >= g && (h %= g)
	}
	d = h % e
	return "http://vpic.video.qq.com/" + d + "/" + a + '.png'
}

console.log(getVideoSnap("o0022uzh8om"));
```

```
<iframe frameborder="0" width="640" height="498" src="https://v.qq.com/iframe/player.html?vid=o0022uzh8om&tiny=0&auto=0" allowfullscreen></iframe>
```

iframe 中的vid