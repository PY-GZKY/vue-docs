> 你知道 `Javascript` 的 `new Date()` 总是不尽人意
 
## 安装
```shell
npm install dayjs --save
cnpm install dayjs --save
```

## 全局引用
```shell
import dayJs from 'dayjs';
Vue.prototype.dayjs = dayjs; //可以全局使用dayjs
```

## 开袋即用
```shell
update_time: this.dayjs().format("YYYY-MM-DD HH:mm:ss"),
```

```shell

methods:{
    getTimeSetting(){
        /*获取当前时间*/
        console.log(this.dayjs().format("YYYY-MM-DD HH:mm:ss"));
        /*将获取到的时间转换为时间戳*/
        console.log(this.dayjs().unix(dayjs().format("YYYY-MM-DD HH:mm:ss")));
        /*将时间戳转为正常时间*/
        let time_ =1613971809;
        console.log(this.dayjs.unix(time_).format('YYYY-MM-DD HH:mm:ss'))
    }

```

> [https://dayjs.fenxianglu.cn](https://dayjs.fenxianglu.cn/)