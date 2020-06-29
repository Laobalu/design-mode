## 代理模式

>代理模式更像是一个中转过程，某些情况下本体对象不能直接访问，需要为本体对象找一个替代对象，以便对本体对象进行访问。代理对象与本体对象接口需要一致，以此达到对本体对象的访问控制。  

比如代购：我们需要去国外买个LV，自己去不了，所以需要代购在国外买了再给你。  

再比如vpn：我们不能直接访问外网，需要代理服务器作为中转。客户端的所有请求先经过代理服务器，由代理服务器再去请求真实服务器。请求成功后，再由代理服务器将真实服务器的响应结果返回客户端。
  
代理模式的变体有多种：保护代理、虚拟代理、缓存代理等等，这里只介绍这常用的三种。
### 保护代理
保护代理主要用于控制不同权限的对象**对本体对象的访问权限**，符合条件的才能访问，起到过滤作用。
```javascript
class Visitor {
  constructor(name, level) {
    this.name = name;
    this.level = level;
  }
  getName() {
    return this.name;
  }
  getLevel() {
    return this.level;
  }
  gotoDancingFloor() {
    console.log('去舞池！');
  }
  gotoRoom() {
    console.log('去包厢！');
  }
}

// 导游（代理用户）
class TourGuide {
  constructor(visitor) {
    this.visitor = visitor;
  }
  getName() {
    this.visitor.getName();
  }
  getLevel() {
    this.visitor.getLevel();
  }
  gotoDancingFloor() {
    if (this.visitor.getLevel() >= 1) {
      console.log('去舞池！');
    } else {
      console.log('无权限！');
    }
  }
  gotoRoom() {
    if (this.visitor.getLevel() >= 3) {
      console.log('去包厢！');
    } else {
      console.log('无权限！');
    }
  }
}

const visitor1 = new Visitor('lilei', 1);
const visitor2 = new Visitor('fofo', 3);

const guide1 = new TourGuide(visitor1)
const guide2 = new TourGuide(visitor2)

guide1.gotoDancingFloor();
guide1.gotoRoom();

guide2.gotoDancingFloor();
guide2.gotoRoom();
```  
本例中游客与导游（游客代理）具有相同接口，游客本身只关心自己的行为。我们调用导游类，在满足条件时，执行与本体相同的代码。这里导游就是保护代理，起到过滤作用。  

### 虚拟代理