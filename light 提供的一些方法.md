# Light提供的一些方法

**推荐使用官方提供的 [light-sdk](http://document.lightyy.com/zh-cn/api/light-sdk/index.html) ，封装都已做好，在Web浏览器开发模式下，不会有报错**

## 壳子内置 LightJSBridge 提供的一些方法
> 适用于使用 LightOS 系统

```
export default {
  getRegistrationID (cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("push.getRegistrationID", {}, function (d) {
        cb(d.data ? d.data : null);
      });
    }else{
      cb(null);
    }
  },
  /*获取壳子的版本*/
  getNativeVersion(cb) {
    // document.addEventListener("deviceready", function () {
    //   LightJSBridge.call("native.version", {}, function (d) {
    //     cb && cb(d.data);
    //   })
    // });
    LightJSBridge.call("native.version", {}, function (d) {
      cb && cb(d.data);
    })
  },
  /*重新打开APP的事件监听并回调*/
  onAppReStart(cb){
    if (window.LightJSBridge) {
      LightJSBridge.onPageAppear = function () {
        cb && cb();
      }
    }
  },
  /*重新从后台唤醒APP*/
  onResumeApp(){
    if (window.LightJSBridge) {
      LightJSBridge.onAppResume = function () {
        cb && cb();
      }
    }
  },
  setAlias (data) {
    if (window.LightJSBridge) {
      LightJSBridge.call('push.setAlias', data, null);
    }
  },
  /**
   * 添加按钮
   * {
        "title": "完成",
        "icon": "",
        "action": "javascript:EventBus.$emit('homePage.showGroup')",
        "position": "right"
      }
   * @param params
   */
  addButton (params) {
    if (window.LightJSBridge) {
      LightJSBridge.call("head.addButton", params || {}, null);
    }
  },

  /**设置背景顶部导航栏背景色
   * params={"color":"#123456"}
   * @param params
   */
  setBackgroundColor (params) {
    if (window.LightJSBridge) {
      LightJSBridge.call("head.setBackgroundColor", params || {}, null);
    }
  },

  sendEvent (event) {
    if (window.LightJSBridge) {
      event = event.replace(/\W+/ig, '_');
      LightJSBridge.call("analytics.sendEvent", {
        "event": event
      }, null);
    }
  },

  /**
   * 改变title文字
   * @param title
   */
  setTitle (title) {
    if (window.LightJSBridge) {
      // document.addEventListener("deviceready", function () {
      //   LightJSBridge.call("head.setTitle", {
      //     title
      //   }, () => {
      //   });
      // });
      LightJSBridge.call("head.setTitle", {
        title
      }, () => {
      });
    }
  },

  /**
   * 移除导航栏按钮
   * params 其中可以设置left和right
   */
  removeButton (params) {
    if (window.LightJSBridge) {
      LightJSBridge.call("head.removeButton", params || {}, null);
    }
  },

  /*回退*/
  back(){
    if (window.LightJSBridge) {
      LightJSBridge.call("native.close", {}, null);
    } else {
      window.history.back();
    }
  },

  /**
   *
   * @param url http://10.26.114.15:3000/#/login
   * @param cb
   */
  open(url){
    if (window.LightJSBridge) {
      window.location.href = 'gmu://web?startPage=' + encodeURIComponent(url + '?open_by_device=true')
    }
  },

  //股票模糊查询
  queryStockFuzzy: function (params, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("quote.wizard", params, function (data) {
        cb(data);
      });
    } else {
      cb({
        "data": [
          {
            "prod_code": "600570.SS",
            "prod_name": "恒生电子",
            "hq_type_code": "XSHG.ESA.M",
            "special_marker": 28
          }
        ]
      })
    }
  },

  //股票行情查询
  queryStockReal: function (params, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("quote.real", params, function (data) {
        cb(data);
      });
    } else {
      cb({
        "data": {
          "data": [{
            "fields": [
              "data_timestamp",
              "shares_per_hand",
              "prod_name",
              "bid_grp",
              "offer_grp",
              "last_px",
              "up_px",
              "down_px"
            ],
            "600570.SS": [
              145959830,
              100,
              "恒生电子",
              "59.68,100,0,59.66,6300,0,59.65,25300,0,59.64,3400,0,59.63,5000,0",
              "59.69,500,0,59.70,57442,0,59.71,100,0,59.72,5200,0,59.73,9200,0",
              59.68,
              68.59,
              56.12
            ]
          }]
        }
      })
    }
  },

  /*添加自选股*/
  addSelfStock: function (params, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("mystock.add", params, function (data) {
        cb(data);
      });
    } else {
      console.log('添加自选股功能正在加载中稍后再试');
    }
  },

  /*查询自选股列表*/
  querySelfStockList: function (params, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("mystock.list", params, function (data) {
        cb(data);
      });
    } else {
      console.log('查询自选股功能正在加载中稍后再试');
    }
  },

  /*删除自选股*/
  delSelfStock: function (params, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("mystock.delete", params, function (data) {
        cb(data);
      });
    } else {
      console.log('删除自选股功能正在加载中稍后再试');
    }
  },
  //选择图片
  choosePic: function (params, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("native.chooseImage", params, function (data) {
        cb && cb(convertBase64ToBlob(data.data.result));
      });
    } else {
      console.log("设备不支持!");
    }
  },

  /**
   * 跳转到股票详情
   * @param stockCode 股票代码（必传）
   * @param stockType 股票类别（如果股票代码如：600570.SS,那么可以不传）
   */
  goToStockDetail: function (stockCode, stockType) {
    var code = '';
    if (!stockCode) {
      console.error('股票代码不能为空');
      return;
    } else {
      code = stockCode.split('.')[0]
    }
    var codeType = '';
    if (stockType) {
      codeType = stockType;
    } else {
      (stockCode.indexOf('SS') >= 0 || stockCode.indexOf('ss') >= 0 || stockCode.indexOf('XSHG') >= 0) && (codeType = 'XSHG');
      (stockCode.indexOf('SZ') >= 0 || stockCode.indexOf('sz') >= 0 || stockCode.indexOf('XSHE') >= 0) && (codeType = 'XSHE');
    }
    window.location.href = 'gmu://stock_detail?stockCode=' + code + '&codeType=' + codeType
  },

  // sendEvent: function (name) {
  //   if (window.LightJSBridge) {
  //     name = name.replace(/\W+/ig, '_');
  //     LightJSBridge.call("analytics.sendEvent", {
  //       "event": name
  //     }, null);
  //   } else {
  //     console.log('数据埋点功能未加载');
  //   }
  // },

  /*设置缓存*/
  setCache: function (data, cb) {
    if (window.LightJSBridge) {
      var length = 0;
      for (var key in data) {
        length++;

        if (data[key] != null) {
          LightJSBridge.call("native.writeData", {
            key: key,
            value: data[key] || {}
          }, function () {
            length--;
            if (length == 0) {
              cb && cb();
            }
          });
        } else {
          this.delCache(key, function () {
            length--;
            if (length == 0) {
              cb && cb();
            }
          });
        }
      }
    } else {
      console.log('页面加载中，请稍后再试');
    }
  },
  /*获取缓存*/
  getCache: function (key, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("native.readData", {
        key: key,
      }, function (data) {
        if (data && data.data && data.data.result) {
          cb && cb(data.data.result)
        } else {
          cb && cb(null);
        }
      });
    } else {
      console.log('页面加载中，请稍后再试');
    }
  },
  /*删除缓存*/
  delCache: function (key, cb) {
    if (window.LightJSBridge) {
      LightJSBridge.call("native.deleteData", {
        "key": key
      }, function () {
        cb && cb();
      });
    } else {
      console.log('页面加载中，请稍后再试');
    }
  }

}

```
