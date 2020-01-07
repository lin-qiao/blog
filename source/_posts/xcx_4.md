---
title: �ġ�С����ҳ���߼�
date: 2018-02-09
tags:
categories: ΢��С����
---



д��ǰ�棺΢��С����֧��ES6�﷨���������߻����ϴ�������ʱ���Զ�תΪES5

### ����ԭ��
С�������ͼ��Ŀǰʹ�� WebView ��Ϊ��Ⱦ���壬���߼������ɶ����� JavascriptCore ��Ϊ���л������ڼܹ��ϣ�WebView �� JavascriptCore ���Ƕ�����ģ�飬�����߱�����ֱ�ӹ����ͨ������ǰ����ͼ����߼�������ݴ��䣬ʵ����ͨ�������ṩ�� evaluateJavascript ��ʵ�֡����û���������ݣ���Ҫ����ת��Ϊ�ַ�����ʽ���ݣ�ͬʱ��ת�������������ƴ�ӳ�һ�� JS �ű�����ͨ��ִ�� JS �ű�����ʽ���ݵ����߶���������

�� evaluateJavascript ��ִ�л��ܺܶ෽���Ӱ�죬���ݵ�����ͼ�㲢����ʵʱ�ġ�


<!-- more -->
### �޸�page���data��ֵ

��vue������ͬ���޸�data������ݱ���ͨ��this.setData���������ʵ�֡�����һ������Ϊ������keyΪdata��߶�Ӧ�ı�������valueΪ������������

```javascript
this.setData({
  key:value
})
```

#### ���ݰ�

WXML �еĶ�̬���ݾ����Զ�Ӧ Page �� data��

* ΢��С��������ݰ��ǵ����ģʽ����WXML�����ݷ����仯��Page��data�����ݲ����ᷢ���仯

* ���������Ҫд�������ڣ�������ʡ��˫����

΢��С���������˫�����ڽ��м򵥵����㣬֧�ֵ������¼��ַ�ʽ��
* ��Ԫ����
* ��������
* �߼��ж�
* �ַ�������
* ����·������

�ٷ��ĵ���https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/data.html

### �б���Ⱦ wx:for

�������ʹ�� wx:for �������԰�һ�����飬����ʹ�������и���������ظ���Ⱦ�������

* Ĭ������ĵ�ǰ����±������Ĭ��Ϊ index�����鵱ǰ��ı�����Ĭ��Ϊ item
* ����ʹ�� `wx:for-index="idx"` ��`wx:for-item="itemName"`���ı�Ĭ��ֵ

```html
<view wx:for="{{array}}">
  {{index}}: {{item.message}}
</view>
```

�ٷ��ĵ���https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/list.html

### ������Ⱦ wx:if
�ڿ���У�ʹ�� wx:if="`{{condition}}`" ���ж��Ƿ���Ҫ��Ⱦ�ô���飺
```html
<block wx:if="{{true}}">
  <view> view1 </view>
  <view> view2 </view>
</block>
```
* <block/> ������һ���������������һ����װԪ�أ�������ҳ�������κ���Ⱦ��ֻ���ܿ������ԡ�

�ٷ��ĵ���https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/conditional.html

### �¼�(bind��catch)

* bind�¼��󶨲�����ֹð���¼�����ð��
* catch�¼��󶨿�����ֹð���¼�����ð��

������а�һ���¼�����������bindtap�����û�����������ʱ����ڸ�ҳ���Ӧ��Page���ҵ���Ӧ���¼���������

``` html
<view id="tapTest" data-hi="WeChat" bindtap="tapName"> Click me! </view>
```

* ע�⣺�¼���������`bindtap="tapName()"`����
* ���η�ʽ�� ��data-��ͷ��������������ַ�-���ӣ������д�д(��д���Զ�ת��Сд)��data-element-type�������� event.currentTarget.dataset �лὫ���ַ�ת���շ�elementType��
* data- ���ݵĲ������Ϳ�������������

�ٷ��ĵ���https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/event.html

### ����ӿ� wx.request

```javascript
wx.request({
  url: 'https://URL',
  data: {},
  method: 'GET', // ����ʽ
  header: {}, // ��������� header
  success: function(res){
    console.log(res.data)
    // ����ɹ��ص�
  },
  fail: function() {
    // ����ʧ�ܻص�
  },
  complete: function() {
    // ��������ص�
  }
})
```

success���ز���˵����

|���� |	����	| ˵��	|
| --------    | :----: |:----: |
|data	|Object/String/ArrayBuffer |	�����߷��������ص�����	|
|statusCode|	Number	|�����߷��������ص� HTTP ״̬��	|
|header|	Object	|�����߷��������ص� HTTP Response Header|

* ���Ҫȡ���������ص����ݣ���`res.data`
* ������statusCode ����һЩ������״̬����ʾ

�ٷ��ĵ��� https://mp.weixin.qq.com/debug/wxadoc/dev/api/network-request.html#wxrequestobject


### ��������

��ʾ���� wx.showToast()
�ر���ʾ���� wx.hideToast()
�����е��� wx.showLoading()
�رռ����е��� wx.hideLoading()
ģ̬���� wx.showModal()

�ٷ��ĵ���https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-react.html
