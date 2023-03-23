```javascript
    //<img id="target" />
    var imgUrl = "https://cdn.staticaly.com/gh/quinhua/pics@main/avaer/jui876rt5dfcvbghy.webp";
    //var el = document.getElementById("target");
    var imgType='webp';//png,jpeg,webp,空为png
    var convertImgToBase64 = (url,type, callback, outputFormat) => {
        var canvas = document.createElement('canvas'),
            ctx = canvas.getContext('2d'),
            img = new Image;
        img.crossOrigin = 'Anonymous';
        img.onload = function () {
            canvas.height = img.height;
            canvas.width = img.width;
            ctx.drawImage(img, 0, 0);
            var dataURL = canvas.toDataURL(outputFormat || `image/${type}`);
            callback.call(this, dataURL);
            canvas = null;
        };
        img.src = url;
    }

    convertImgToBase64(imgUrl,imgType, function (base64Img) {
        //el.setAttribute("src", base64Img);
        console.log(base64Img);
    });
```