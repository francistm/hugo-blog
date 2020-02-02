+++
date = "2016-03-21T00:00:00+08:00"
slug = "parse-json-in-golang"
title = "Go中解析JSON"
lastmod = "2016-03-28T06:53:11+08:00"
publishDate = "2016-03-21T16:07:00+08:00"
+++

最近做了一个小的Socket应用，考虑到C已经忘干净了，Ruby解释型语言又显不出档次，就试试Go吧。

Socket中的数据想想什么方法比较好呢，XML已经被上次的一个破SOAP接口搞疯了；自己搞一套格式又重复造轮子，干脆直接发JSON好了。

参考了下官方的[JSON and Go](http://golang.org/blog/json-and-go)，似乎有两种方法，一个 `interface{}` 的比较灵活；另一种 struct 的比较固定。用惯了解释语言，当然先爽爽 `interface{}` 的吧。

JSON格式如下

~~~json
{
  "command": "INIT",
  "properties": {
    "id": "TEST-CLIENT",
    "relays": [
      {
        "id": "RELAY-1",
        "ip": "192.168.0.101",
        "nodes": [
          {
            "id": "NODE-1",
            "desc": "First Node in Relay 1"
          },
          {
            "id": "NODE-2",
            "desc": "Second Node in Relay 1"
          }
        ]
      },
      {
        "id": "RELAY-2",
        "ip": "192.168.0.102",
        "nodes": [
          {
            "id": "NODE-1",
            "desc": "First Node in Relay 2"
          },
          {
            "id": "NODE-2",
            "desc": "Second Node in Relay 2"
          }
        ]
      }
    ]
  }
}
~~~

`properties`下有两个`relay`对象，而`relay`对象下又有两个`node`对象。

~~~go
func decodeJson(json []byte) (interface{}, error) {
  var data interface{}

  if err := json.Unmarshal(json, &data); err != nil {
    return data, err
  }

  // 访问 command 属性
  command := data.(map[string]interface{})["command"].(string)

  // 访问 properties
  properties := data.(map[string]interface{})["properties"].(map[string]interface{})

  // 访问所有 relays
  relays := properties["relays"].([]interface{})

  // 访问 relays[0].nodes
  nodesInRelay0 := relays[0].(map[string]interface{})["nodes"].([]interface{})

  return data, nil
}
~~~

这简直是坑啊，代码长到变态，而且遇到传入的JSON缺少某个参数的时候程序就会出错，而编译器无法检查到这种错误。看来稳妥起见还是用`struct`的方法把。

~~~go
type CmdData struct {
  Cmd string `json:"command"`
  Properties []struct {
    Id string `json:"id"`
    Relays []struct {
      Id string `json:"id"`
      Ip string `json:"ip"`
      Nodes []struct {
        Id string `json:"id"`
        Desc string `json:"desc"`
      } `json:"nodes"`
    } `json:"relays"`
  } `json:"properties"`
}

 
func decodeJson(json []byte) (*CmdData, error) {
  cmdData := &CmdData{}
  
  if err := json.Unmarshal(json, cmdData); err != nil {
    return nil, err
  }

  // 访问 command 属性
  command := cmdData.Cmd

  // 访问 properties 属性
  properties := cmdData.Properties

  // 访问所有 relays
  relays := cmdData.Properties.Relays

  // 访问 relays[0].nodes
  nodes := cmdData.Properties.Relays[0].Nodes

  return cmdData, nil
}
~~~

虽然前面定义了多个结构体，但不仅访问起来简单许多，解析时也不会因为缺少某个参数而异常退出。

几点需要注意的问题

- 可以通过结构体后中属性名后面的`field name`来定义该属性在JSON中相对应的名称
- JSON中的所有数字类型在Go中都会被转换为`float64`，这个错误不会被编译器检查出
- 结构体的属性名必须以大写开头，不然`Unmarshal`不会尝试着充填这个字段，因为从包外部无法访问到
