```js
import { ElMessageBox, ElMessage } from 'element-plus'
/**
 * 消息提示
 * @param message 提示信息
 * @param type 消息类型 [默认为success,可选success/warning/info/error]
 * @param duration 显示时间 [默认为3000]
 */
const messages = (message, type, duration) => {
    ElMessage({
        showClose: true,
        message,
        duration,
        type
    });
}
const qm = (message, type = "success", duration = 3000) => {
    messages(message, type, duration);
}

/**
 * 通知
 * @param title 公通知标题
 * @param message 消息内容
 * @param type 消息类型 [默认为success,可选success/warning/info/error]
 * @param position 弹出位置 [默认为 left-right,可选top-right/top-left/bottom-right/bottom-left]
 * @param duration 显示时间 [默认为3000]
 */
const notification = (message,title,type, position, duration) => {
    ElNotification({
        message,
        title,
        type,
        duration,
        position
    })
}
const qn = (message, title, type = "success", position = "top-right", duration = 3000) => {
    notification(message, title, type, position, duration)
}

export {
    qm,
    qn
}
```

