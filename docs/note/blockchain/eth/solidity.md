<!-- solidity智能合约编写学习笔记-->

# 1.Solidity简介

Solidity是一种用于以太坊智能合约编写的面向对象高级编程语言。

以下是Solidity语言的详细教程，包括基础语法和代码示例以及解释。

# 2.基础语法

## 数据类型

Solidity支持以下数据类型：

- 布尔类型：`bool`
- 整数类型：`int`, `uint`
- 固定大小字节数组类型：`bytes1` 到 `bytes32`
- 动态大小字节数组类型：`bytes`
- 字符串类型：`string`
- 地址类型：`address`
- 合约类型：`contract`
- 数组类型：`<type>[]` 或 `<type>[<length>]`
- 映射类型：`mapping(<keyType> => <valueType>)` 

## 变量声明

变量可以使用`var`或`<type>`进行声明。在Solidity中，声明变量时必须显式指定变量类型。

```solidity
uint256 public myVar = 123; // 声明一个公共的无符号整数类型变量，初始值为123
```

## 函数

函数定义的语法如下：

```solidity
function functionName(<parameter types>) <visibility> <returns> {
    // 函数体
}
```

- `<parameter types>`是函数参数的类型列表
- `<visibility>`是函数可见性（`public`、`private`、`external` 或 `internal`）
- `<returns>`是返回值的类型。

以下是一个简单的函数示例：

```solidity
function add(uint256 x, uint256 y) public returns (uint256) {
    return x + y;
}
```

## 构造器

在 Solidity 中，`constructor` 是一种特殊的函数，它在合约被创建时自动调用，用于初始化合约的状态。与其他函数不同的是，`constructor` 函数只会在合约创建时执行一次。它可以接受参数，并且可以用于初始化合约中的变量、数组、结构体等状态。

下面是一个简单的 Solidity 合约，其中包含一个构造函数：

```solidity
pragma solidity ^0.8.0;

contract MyContract {
    uint256 public myNumber;

    constructor(uint256 _myNumber) {
        myNumber = _myNumber;
    }
}
```

在上面的代码中，我们定义了一个 `MyContract` 合约，并且在合约中声明了一个 `myNumber` 的公共变量。在构造函数中，我们将一个 `uint256` 类型的 `_myNumber` 参数赋值给 `myNumber`。

在创建合约实例时，我们可以向构造函数传递参数。例如，如果我们想要创建一个 `MyContract` 合约，使得 `myNumber` 的值为 `42`，我们可以这样做：

```solidity
MyContract myContract = new MyContract(42);
```

在上面的代码中，我们使用 `new` 关键字创建了一个 `MyContract` 的实例，并且向构造函数传递了参数 `42`。这样，当合约被创建时，`myNumber` 的值就会被初始化为 `42`。

## 事件

事件用于在合约中记录日志以及监听，方便后续的追踪和分析。

```solidity
event Transfer(address indexed _from, address indexed _to, uint256 _value);

function transfer(address _to, uint256 _value) public {
    require(balanceOf[msg.sender] >= _value);

    balanceOf[msg.sender] -= _value;
    balanceOf[_to] += _value;

    emit Transfer(msg.sender, _to, _value);
}
```

### 监听机制

Solidity 支持事件（Event）的监听机制。

事件是一种合约内部的通信机制，可以在合约的某些重要状态变化时发出事件。合约外部的应用程序可以监听事件，以获取有关合约状态变化的信息。

事件通常定义在合约顶层，并由 `event` 关键字声明。事件可以包含任意数量和类型的参数，当事件被触发时，这些参数的值将被记录在区块链上。

下面是一个简单的 Solidity 合约，其中定义了一个名为 `MyEvent` 的事件：

```solidity
pragma solidity ^0.8.0;

contract MyContract {
    event MyEvent(uint256 indexed value1, string value2);

    function myFunction(uint256 _value1, string memory _value2) public {
        // 触发事件并传递参数
        emit MyEvent(_value1, _value2);
    }
}
```

在上面的合约中，我们定义了一个名为 `MyEvent` 的事件，它包含两个参数：一个无索引的 `uint256` 类型的参数 `value1` 和一个字符串类型的参数 `value2`。在 `myFunction` 函数中，我们触发了该事件，并传递了两个参数。

在合约外部，我们可以使用 Web3.js 等库来监听事件。下面是一个使用 Web3.js 监听上面合约中的 `MyEvent` 事件的示例代码：

```javascript
const MyContract = artifacts.require("MyContract");

contract("MyContract", accounts => {
  it("should emit MyEvent event", async () => {
    const instance = await MyContract.deployed();

    const value1 = 42;
    const value2 = "hello world";

    const event = instance.MyEvent({ value1 }, { fromBlock: 0, toBlock: "latest" });

    // 监听事件
    event.on("data", event => {
      assert.equal(event.returnValues.value1, value1);
      assert.equal(event.returnValues.value2, value2);
    });

    // 触发事件
    await instance.myFunction(value1, value2);
  });
});
```

在上面的代码中，我们使用 Web3.js 的 `event` 方法来监听合约中的 `MyEvent` 事件。当事件被触发时，我们可以使用 `data` 事件来获取事件的详细信息，并进行必要的处理。

总之，Solidity 中支持事件监听机制，我们可以通过定义事件来实现合约内部的通信，并使用 Web3.js 等库来监听事件并获取合约状态的变化。

## 修饰器

修饰器是用于在函数执行前或执行后添加额外逻辑的函数。在Solidity中，修饰器通常用于验证函数参数或权限。

```solidity
modifier onlyOwner {
    require(msg.sender == owner);
    _;
}

function changeOwner(address _newOwner) public onlyOwner {
    owner = _newOwner;
}
```

上述示例中，`onlyOwner`修饰器用于限制`changeOwner`函数的执行权限，只有合约的拥有者才能调用该函数。

# 3.特性

## 变量的生命周期

在 Solidity 中，变量的生命周期是根据它们的作用域和存储位置来决定的。

首先，我们需要了解 Solidity 中的变量存储位置。Solidity 中的变量存储位置有三种：

- memory：内存存储，只在函数调用期间存在，函数调用结束后变量就会被销毁。
- storage：存储器存储，将变量的值永久存储在区块链上。
- calldata：类似于 memory，但它只能用于函数参数和返回值，不能在函数内部赋值或修改。

在 Solidity 中，变量的生命周期也取决于其作用域。在合约内部，有四种作用域：

- State variables：状态变量，定义在合约顶层的变量，其作用域为整个合约，存储在区块链的存储器中，其生命周期随着合约的存续而持久存在。
- Function arguments and variables：函数参数和局部变量，它们的作用域只在函数内部，存储在内存或栈中，随着函数的执行结束而被销毁。
- Function return variables：函数返回值变量，它们的作用域也只在函数内部，但是如果它们被定义为状态变量，那么它们的生命周期就与合约一样长了。
- Global variables：全局变量，它们的作用域跨越整个 Solidity 文件，存储在内存或栈中，其生命周期随着文件的编译而开始，随着程序的结束而结束。

下面是一个简单的 Solidity 合约，用于演示变量的生命周期：

```solidity
pragma solidity ^0.8.0;

contract MyContract {
    uint256 public myStateVariable; // 状态变量

    function myFunction(uint256 _argument) public {
        uint256 myLocalVariable = _argument;

        // 访问状态变量并修改其值
        myStateVariable = 42;

        // 返回值变量
        uint256 myReturnValue = 123;

        // 可以在函数内部定义全局变量
        bool globalVariable = true;

        // do something...
    }
}
```

在上面的代码中，我们定义了一个 `MyContract` 合约，其中包含一个状态变量 `myStateVariable` 和一个名为 `myFunction` 的函数。在 `myFunction` 函数中，我们定义了一个局部变量 `myLocalVariable`，并将函数的参数 `_argument` 赋值给它。同时，我们也定义了一个返回值变量 `myReturnValue` 和一个全局变量 `globalVariable`。

总之，在 Solidity 中，变量的生命周期是根据它们的作用域和存储位置来决定的。状态变量的生命周期与合约的生命周期一样长，而函数参数和局部变量的生命周期只在函数执行期间存在，全局变量的生命周期与文件编译期间存在。因此，在编写 Solidity 合约时，我们需要注意变量的作用域和存储位置，以便正确地管理变量的生命周期，避免不必要的资源浪费和错误。

> [!WARNING]
>
> Solidity 中的存储器和内存都是有限的资源，因此需要谨慎使用。如果在函数内部定义的变量较多或者变量的值比较大，可能会导致函数栈或内存不足，从而抛出异常。为了避免这种情况，我们可以考虑使用状态变量或通过优化算法来减少变量的使用。

# 4.智能合约示例

## 管理代币

```solidity
pragma solidity ^0.8.0;

contract MyToken {
	// 代币的基本属性
    string public name;
    string public symbol;
    uint256 public totalSupply;
	// 余额
    mapping(address => uint256) public balanceOf;
    // 代币授权关系
    mapping(address => mapping(address => uint256)) public allowance;
	
	// 记录代币转移
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    // 记录代币授权
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);

	constructor(string memory _name, string memory _symbol, uint256 _totalSupply) {
    	name = _name;
    	symbol = _symbol;
    	totalSupply = _totalSupply;
    	balanceOf[msg.sender] = _totalSupply;
	}

	function transfer(address _to, uint256 _value) public {
    	require(balanceOf[msg.sender] >= _value);

    	balanceOf[msg.sender] -= _value;
    	balanceOf[_to] += _value;

    	emit Transfer(msg.sender, _to, _value);
	}

	function approve(address _spender, uint256 _value) public returns (bool) {
    	allowance[msg.sender][_spender] = _value;

    	emit Approval(msg.sender, _spender, _value);

    	return true;
	}

	function transferFrom(address _from, address _to, uint256 _value) public {
    	require(balanceOf[_from] >= _value);
    	require(allowance[_from][msg.sender] >= _value);

    	balanceOf[_from] -= _value;
    	balanceOf[_to] += _value;

    	allowance[_from][msg.sender] -= _value;

    	emit Transfer(_from, _to, _value);
	}
}
```

以上智能合约实现了一个简单的代币合约，其中包括：

- `name`、`symbol`、`totalSupply` 代币的基本属性。
- `balanceOf` 用于存储各个账户的余额。
- `allowance` 用于记录各个账户之间的代币授权关系。
- `Transfer` 和 `Approval` 两个事件用于记录代币转移和授权操作。
- `transfer`、`approve` 和 `transferFrom` 三个函数用于代币转移和授权操作。其中 `transfer` 用于账户之间的直接转移，`approve` 用于账户之间的授权，`transferFrom` 则用于授权后代币的转移。注意，在 `transferFrom` 函数中，需要对转移金额进行检查，以确保账户余额和授权金额的正确性。

以上是Solidity语言的详细教程和一个简单的代码示例。需要注意的是，在实际应用中，合约的复杂度和功能会更加丰富，需要更加全面和细致的考虑。

## Shamri秘密共享

以下是一个使用Solidity实现Shamir秘密共享算法的智能合约示例：

```solidity
pragma solidity ^0.8.0;

contract ShamirSecretSharing {
    struct Point {
        uint x;
        uint y;
    }

    // 共享秘密值的总数和阈值
    uint public n;
    uint public k;

    // 多项式的系数和常数项
    uint[] private coefficients;
    uint private constantTerm;

    // 随机数生成器
    uint private seed;

    constructor(uint _n, uint _k, uint _secret) {
        require(_k > 0 && _k <= _n, "Invalid parameters");
        n = _n;
        k = _k;

        coefficients.push(_secret);
        seed = uint(keccak256(abi.encodePacked(msg.sender, block.number)));
        for (uint i = 1; i < k; i++) {
            coefficients.push(randomNumber());
        }
        constantTerm = randomNumber();
    }

    function randomNumber() private returns (uint) {
        seed = uint(keccak256(abi.encodePacked(seed)));
        return seed % 256;
    }

    function evaluate(uint x) private view returns (uint) {
        uint result = constantTerm;
        uint xPower = 1;
        for (uint i = 0; i < k; i++) {
            result += coefficients[i] * xPower;
            xPower *= x;
        }
        return result;
    }

    function share(uint[] memory xs) public view returns (Point[] memory) {
        require(xs.length == n, "Invalid input length");
        Point[] memory points = new Point[](n);
        for (uint i = 0; i < n; i++) {
            points[i] = Point(xs[i], evaluate(xs[i]));
        }
        return points;
    }

    function recover(Point[] memory points) public view returns (uint) {
        require(points.length == k, "Invalid input length");
        uint result = 0;
        for (uint i = 0; i < k; i++) {
            uint numerator = 1;
            uint denominator = 1;
            for (uint j = 0; j < k; j++) {
                if (i == j) {
                    continue;
                }
                numerator *= points[j].x;
                denominator *= points[j].x - points[i].x;
            }
            result += points[i].y * numerator * inverseMod(denominator);
        }
        return result;
    }

    function inverseMod(uint a) private view returns (uint) {
        require(a != 0, "Division by zero");
        uint q = 0;
        uint newQ = 1;
        uint r = 1;
        uint newR = 0;
        uint temp;
        uint m = 256;
        while (a != 0) {
            uint quotient = r / a;
            (r, newR) = (newR, r - quotient * a);
            (q, newQ) = (newQ, q - quotient * newQ);
            (a, temp) = (temp, a);
        }
        if (r > 1) {
            return m - r;
        }
        if (q < 0) {
            return q + m;
        }
        return q;
    }
}
```

在此智能合约中，我们定义了一个名为`ShamirSecretSharing`的合约，并使用以下变量来存储秘密共享算法的参数和状态：

- `n`：共享秘密值的总数
- `k`：阈值，即恢复秘密值所需的最少共享秘密值的数量
- `coefficients`：多项式的系数，其中第一个系数为秘密值
- `constantTerm`：多项式的常数项
- `seed`：随机数生成器的种子

合约的构造函数接受三个参数：`_n`、`_k`和`_secret`。它首先验证`_k`大于0且小于等于`_n`，然后使用`_secret`初始化多项式的第一个系数，并使用随机数生成器生成其余的系数和常数项。

随机数生成器使用了keccak256哈希函数，它接受一个种子作为输入，并根据该种子生成一个随机数。在此实现中，我们使用了一个简单的线性同余算法来生成伪随机数。

`evaluate`函数接受一个参数`x`，返回多项式在x处的值。它首先计算常数项，然后对于每个系数，计算其乘以x的幂并将其相加。

`share`函数接受一个长度为n的数组`xs`，返回一个长度为n的数组，其中每个元素都是(x, y)坐标对，表示多项式在x处的值。它遍历数组xs中的每个元素，并对于每个元素计算多项式在该元素处的值。最后，它返回一个包含所有(x, y)坐标对的数组。

`recover`函数接受一个长度为k的数组`points`，其中每个元素都是(x, y)坐标对，表示多项式在x处的值。它使用拉格朗日插值法恢复多项式的系数，并返回多项式的第一个系数，即秘密值。

`inverseMod`函数接受一个整数a，计算a模256的乘法逆元，并返回结果。它使用扩展欧几里得算法计算乘法逆元。如果a模256不可逆，则抛出异常。

需要注意的是，在Solidity中，整数运算默认是有符号的，因此我们需要确保在运算过程中没有出现溢出或下溢的情况。我们还使用`require`函数来确保函数的输入参数和状态变量满足预期的条件。