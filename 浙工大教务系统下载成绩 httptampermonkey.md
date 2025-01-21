```JavaScript
// ==UserScript==
// @name         浙工大教务系统下载成绩
// @namespace    http://tampermonkey.net/
// @version      0.5
// @description  This is a magic that can help you check the composition of the final evaluation.
// @author       Jesse
// @match        把地址粘贴到这里。
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    let downloadLink = document.createElement('a');
    downloadLink.style.display = 'none';
    document.body.appendChild(downloadLink);

    let ele = $("<button type='button' class='btn btn-default btn_dc' href='javascript:void(0);'><i class='bigger-100 glyphicon glyphicon-export'></i> 导出所有成绩</button>");

    let popupHtml = `
        <div id="popup-window" style="display:none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 1000;">
            <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: white; padding: 20px; border-radius: 10px; text-align: center;">
                <h3>彦祖亦菲们给个关注呀！</h3>
                <p>我的小红书: <strong>Jesse-小杰要变强</strong></p>
                <p>我的小红书账号ID: <strong>5865726308</strong></p>
                <img id="qrcode" src="" alt="二维码" style="margin-top: 10px; max-width: 200px; max-height: 200px;">
                <p>扫码关注我，获取更多信息！</p>
                <button id="close-popup" style="margin-top: 20px;">关闭</button>
            </div>
        </div>
    `;

    $('body').append(popupHtml);

    let xiaohongshuUrl = 'https://www.xiaohongshu.com/user/profile/64f5ce750000000005003deb?xsec_token=YBcrOuHUQR_V6ZrHvRRu0LvTi3lqS23UmsZiZrSdwkqiY=&xsec_source=app_share&xhsshare=CopyLink&appuid=64f5ce750000000005003deb&apptime=1737450348&share_id=24fd06b6a2d64053b3d38d805df5774b'; 
    let qrCodeUrl = `https://api.qrserver.com/v1/create-qr-code/?data=${encodeURIComponent(xiaohongshuUrl)}&size=200x200`;

    document.getElementById('qrcode').src = qrCodeUrl;

    $('#close-popup').click(function() {
        $('#popup-window').hide();
    });

    ele.click(function() {
        $('#popup-window').show();

        function downFile(blob) {
            downloadLink.download = new Date().getTime() + ".xlsx";
            downloadLink.href = URL.createObjectURL(blob);
            downloadLink.click();
            URL.revokeObjectURL(downloadLink.href);
        }

        var xhr = new XMLHttpRequest();
        xhr.open("POST", '/jwglxt/cjcx/cjcx_dcXsKccjList.html', true);
        xhr.responseType = 'blob';
        xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");

        let xnm = encodeURIComponent(document.querySelectorAll('#xnm')[0].value);
        let xqm = encodeURIComponent(document.querySelectorAll('#xqm')[0].value);
        let data = "gnmkdmKey=N305005&xnm=" + xnm + "&xqm=" + xqm + "&dcclbh=JW_N305005_GLY&exportModel.selectCol=kcmc%40%E8%AF%BE%E7%A8%8B%E5%90%8D%E7%A7%B0&exportModel.selectCol=xnmmc%40%E5%AD%A6%E5%B9%B4&exportModel.selectCol=xqmmc%40%E5%AD%A6%E6%9C%9F&exportModel.selectCol=kkbmmc%40%E5%BC%80%E8%AF%BE%E5%AD%A6%E9%99%A2&exportModel.selectCol=kch%40%E8%AF%BE%E7%A8%8B%E4%BB%A3%E7%A0%81&exportModel.selectCol=jxbmc%40%E6%95%99%E5%AD%A6%E7%8F%AD&exportModel.selectCol=xf%40%E5%AD%A6%E5%88%86&exportModel.selectCol=xmcj%40%E6%88%90%E7%BB%A9&exportModel.selectCol=xmblmc%40%E6%88%90%E7%BB%A9%E5%88%86%E9%A1%B9&exportModel.exportWjgs=xls&fileName=%E6%96%87%E4%BB%B91656485751290";

        xhr.onload = function() {
            if (xhr.status === 200) {
                downFile(xhr.response);
            } else {
                console.error("请求失败，状态码：" + xhr.status);
            }
        };

        xhr.onerror = function() {
            console.error("请求发生错误");
        };

        xhr.ontimeout = function() {
            console.error("请求超时");
        };

        xhr.timeout = 5000; 

        xhr.send(data);
    });

    $('body').append(ele);
})();

```

