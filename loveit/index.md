# LoveIt


[TOC]

# 美化

### 添加运行时间
1. 拷贝  \themes\LoveIt\layouts\partials\footer.html
	到	\layouts\partials\footer.html，
	在  `<div class="footer-container">`  的下方添加:
	```html
	<div class="footer-line">
		<span id="run-time"></span>
	</div>
	```
2. 修改custom.js
	```html
	/* 站点运行时间 */
	function runtime() {
		window.setTimeout("runtime()", 1000);
		/* 请修改这里的起始时间 */
		let startTime = new Date('04/24/2018 15:00:00');
		let endTime = new Date();
		let usedTime = endTime - startTime;
		let days = Math.floor(usedTime / (24 * 3600 * 1000));
		let leavel = usedTime % (24 * 3600 * 1000);
		let hours = Math.floor(leavel / (3600 * 1000));
		let leavel2 = leavel % (3600 * 1000);
		let minutes = Math.floor(leavel2 / (60 * 1000));
		let leavel3 = leavel2 % (60 * 1000);
		let seconds = Math.floor(leavel3 / (1000));
		let runbox = document.getElementById('run-time');
		runbox.innerHTML = '本站已运行<i class="far fa-clock fa-fw"></i> '
			+ ((days < 10) ? '0' : '') + days + ' 天 '
			+ ((hours < 10) ? '0' : '') + hours + ' 时 '
			+ ((minutes < 10) ? '0' : '') + minutes + ' 分 '
			+ ((seconds < 10) ? '0' : '') + seconds + ' 秒 ';
	}
	runtime();
	```
	###
