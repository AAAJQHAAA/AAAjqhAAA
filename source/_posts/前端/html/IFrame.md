---
title: IFrame
date: 2024-02-05 15:46:45
category: 前端
tags:
  - html标签
  - IFrame
---

# HTMLIFrameElement

```js
// 提供特殊的属性和方法(除了HTMLElement接口之外，它还可以通过继承获得这些属性和方法)，用于操作内联框架元素的布局和表示。
interface HTMLIFrameElement extends HTMLElement {
    /**
     * 设置或检索对象与相邻文本的对齐方式。
     */
    /** @弃用 */
    align: string;
    allow: string;
    allowFullscreen: boolean;
    allowPaymentRequest: boolean;

    
    readonly contentDocument: Document | null;// 检索页或框架的文档对象
    readonly contentWindow: WindowProxy | null;// window对象

    /** @弃用 */
    frameBorder: string;// 设置或检索是否为框架显示边框

    height: string;// 高度

    /** @弃用 */
    longDesc: string;// 为对象的长描述设置或检索URI
    /** @弃用 */
    marginHeight: string;// 设置或检索在框架中显示文本之前的上下边距高度
    /** @弃用 */
    marginWidth: string;// 设置或检索在框架中显示文本之前的左右边距宽度

    name: string;// 名称
    referrerPolicy: ReferrerPolicy;
    readonly sandbox: DOMTokenList;

    /** @弃用 */
    scrolling: string;// 是否可以滚动

    src: string;// 加载的URL。

    srcdoc: string;// 包含的页的内容
    width: string;// 对象的宽度
    getSVGDocument(): Document | null;
}
```

