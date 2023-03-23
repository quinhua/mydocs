# 十六进制转换为RGB
```javascript
       const hexToRGB = hex => {
        let alpha = false,
            h = hex.slice(hex.startsWith('#') ? 1 : 0);
        if (h.length === 3) h = [...h].map(x => x + x).join('');
        else if (h.length === 8) alpha = true;
        h = parseInt(h, 16);
        return (
            'rgb' +
            (alpha ? 'a' : '') +
            '(' +
            (h >>> (alpha ? 24 : 16)) +
            ', ' +
            ((h & (alpha ? 0x00ff0000 : 0x00ff00)) >>> (alpha ? 16 : 8)) +
            ', ' +
            ((h & (alpha ? 0x0000ff00 : 0x0000ff)) >>> (alpha ? 8 : 0)) +
            (alpha ? `, ${h & 0x000000ff}` : '') +
            ')'
        );
    };
    console.log(hexToRGB('#27ae60'));//rgba(39, 174, 96)
    console.log(hexToRGB('27ae60'));//rgb(39, 174, 96)
    console.log(hexToRGB('#7b2243'));//rgb(123, 34, 67)
    console.log(hexToRGB('#fff')); //rgb(255, 255, 255)
```

# RGB转换为十六进制
```javascript
    const RGBToHex = (r, g, b) => {
        return "#" + ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0');
    }
    console.log(RGBToHex(39, 174, 96)); // #27ae60
    console.log(RGBToHex(123,34,67));//#7b2243
    console.log(RGBToHex(255, 255, 255));//#ffffff
```

# 随机生成随机十六进制颜色
```javascript
        function generateRandomHexColor() {
            let colorGenerated = "#" + (Math.random() * 0xfffff * 1000000).toString(16).slice(0, 6);
            if (colorGenerated !== "#0000ff" && colorGenerated !== "#ff0000") {
                return colorGenerated;
            }
            colorGenerated = "#" + (Math.random() * 0xfffff * 1000000).toString(16).slice(0, 6);
        }
        console.log(generateRandomHexColor())
```