# RQ1：智能合约语言语法定义和业界实践习惯

本文将RQ1的研究过程拆分为两个子任务，一是对比11种主流合约语言的语法，归纳其核心操作集合；二是分析20个业界智能合约项目，总结开发者的实践习惯。

## 语法定义

对于第一个子任务，本小节对比了现有11种主流智能合约语言在关键字、数据类型、内置变量和内置函数方面的语法定义，旨在为后续在Java语言基础上定义智能合约领域语法提供参考依据。

### 关键字

关键字是指在编程语言中具有特定含义并且被保留用于特定目的的单词或符号。在此，本文仅考虑智能合约场景下的关键字。本文参考相关文献，建立了最基础的智能合约元模型，如图1所示。SmartContract类表示智能合约，ImportPackage类表示导入包，Struct类表示结构体，Function类表示函数，Property类表示属性。智能合约由导入包、结构体、函数和属性组成。Modifier类表示修饰符，Event类表示事件，两者都是函数的子类。从智能合约元模型中提取核心元素作为智能合约领域特定的关键字。11种智能合约开发语言对于上述关键字定义的语法对比如表1所示。接下来，逐一阐述各语言对表中每列涉及关键字的语法定义。

图1：

<img src="/Users/bugfrog/Desktop/typora_images/智能合约元模型.png" alt="智能合约元模型" style="zoom: 25%;" />

表1：

{\footnotesize
\begin{longtable}[h]{m{29pt} m{25pt} m{28pt} m{19pt} m{78pt} m{29pt} m{20pt} m{40pt} m{36pt}}
    \caption{语言关键字对比} 
    \label{表-语言关键字对比} \\
         \toprule    
         语言&合约&结构体&事件&修饰符&函数&导入&变量&常量 \\
        \hline
        Java&class&class&/&public、protected、default、private、static、final、abstract&/&import&[类型] [名称]&final [类型] [名称] \\
        \hline
        Go&type [名称] struct&type [名称] struct&/&go、defer&func&import&var [名称] [类型]&const [名称] [类型] \\
        \hline
        Solidity&contract&struct&event&public、private、internal、external、payable、view、pure、storage、memory&function&import&[类型] [名称]&[类型] constant [名称] \\
        \hline
       Vyper&contract&struct&event&public、private、external、payable、constant、external&def&import&[名称]&[名称] \\
        \hline
        DAML&template&data&/&/&choice&import&let [名称]: [类型]&/ \\
        \hline
        Move&module&struct&/&public、friend、private、entry&fun&import&let [名称]: [类型]&const [名称]: [类型] \\
        \hline
        Liquid&mod&struct&event&pub&fn&use&let [名称]: [类型]&const [名称]: [类型] \\
        \hline
        Liquidity&/&type&/&/&let\%entry&/&let [名称]&/ \\
        \hline
        Flint&contract&struct&event&public、mutating&func&/&[类型] [名称]&/ \\
        \hline
        Serpent&/&data&/&/&def&import&[名称]&[名称] \\
        \hline
        Mandala&module&type&/&default、public、private、mint、active、init、dependent&/&import&[名称]&val [名称] \\
        \bottomrule 
    \end{longtable} 
}

- 智能合约的声明。Java和Go作为通用编程语言，通过构造特殊类或结构体的方式，使该类或结构体具备合约特性。不同的智能合约DSL大多各自使用不同的关键字声明合约，其中使用contract关键字居多。少数DSL没有合约声明相关的关键字，直接以文件作为一个合约。
- 结构体的声明。Java中没有结构体这一概念，但可以通过类实现结构体的功能。而其他语言，包括Go和所有的智能合约DSL，针对结构体都有专门的关键字，以struct居多。
- 事件的声明。事件本质上是一种特殊的函数，可以在合约的逻辑处理中触发事件，使事件的名称和相关数据广播到区块链网络中，以便其他参与者进行监听和处理，比如将信息记录到数据库中或者触发其他相关的业务操作，从而实现更加灵活和复杂的智能合约应用。在半数的智能合约DSL中存在事件这一概念。
- 修饰符。本文关注的修饰符主要分为两类，一类是访问权限修饰符，另一类是合约功能修饰符。在访问权限修饰符方面，Java中的public、protected、default、private，Solidity中的public、external、internal、private，其可见性均依次递减。在合约功能修饰符方面，Solidity设计了一些与账本状态读写相关的函数修饰符，其中payable修饰的函数表示具备账本读取和修改的功能，view修饰函数表示读取账本而不修改账本，pure修饰函数表示既不修改账本也不读取账本。Solidity还提供了两种和存储方式相关的变量修饰符storage、memory。此外，Mandala提供init修饰符用于标注合约的初始化函数。其他智能合约DSL的修饰符所具备的作用已经囊括在前者中，只是使用的关键字有所不同。
- 函数的声明。上述语言声明函数的关键字各有不同，大多数都是围绕函数的英文单词或缩写。而Java声明函数没有特定关键字，只需要特定修饰符，以及按固定格式明确参数列表和返回值的数据类型即可。
- 导入的声明。无论是通用编程语言还是智能合约DSL，大多数语言导入第三方包或库的关键字都是import。
- 变量和常量的声明。上述语言可以分为动态类型语言和静态类型语言两类，因此声明变量的方式也普遍分为两类。动态类型语言声明变量时不需要指明数据类型，而静态类型语言需要。对于同一种语言而言，其常量的声明与变量类似，或使用的关键字不同，或需要额外的关键字，或与变量声明方式完全一致。

### 数据类型

数据类型通常用于在代码中声明变量、函数参数和返回值等，以区分数据的类型。不同的数据类型有不同的操作和限制，而不同的编程语言可能具备不同的数据类型。由于Mandala是一种处于研究发展阶段的语言，目前收集到的数据类型方面的相关资料十分有限，总结可能不全面，因此不在此讨论。其他10种智能合约开发语言在常见数据类型方面的对比如表2所示。

表2：

{\footnotesize
\begin{longtable}[h]{m{29pt} m{18pt} m{30pt} m{28pt} m{23pt} m{23pt} m{28pt} m{20pt} m{27pt} m{25pt} m{30pt}}
    \caption{语言数据类型对比} 
    \label{表-语言数据类型对比} \\
         \toprule    
         语言&位&整型&浮点型&布尔&字符串&地址&枚举&数组&映射&时间 \\
        \hline
        Java&byte&short、int、long&float、double&boolean&String&/&Enum&Array、List&Map&Date \\
        \hline
        Go&byte&uint、int&float&bool&string&/&/&[n]type&map&Time \\
        \hline
        Solidity&byte、bytes&uint、int&/&bool&string&address&enum&type[n]&mapping&/ \\
        \hline
        Vyper&bytes&uint、int、wei\_value&/&bool&String&address&/&type[n]&t1[t2]&/ \\
        \hline
        DAML&/&Int&Decimal&Bool&Text&Party&/&List&Map&Date、Time\\
        \hline
        Move&/&u8&/&boolean&string&address&/&vector&/&/ \\
        \hline
        Liquid&bytes&u8、i8&/&bool&String&Address&/&Vec&Mapping&/ \\
        \hline
        Liquidity&bytes&int、nat&dun&bool&string&address&/&list&map&timestamp \\
        \hline
       Flint&/&Int&/&Bool&String&Address&enum&Array&[t1:t2]&/\\
       \hline
       Serpent&/&int&/&bool&string、text&/&/&array&/&/\\
        \bottomrule 
    \end{longtable} 
}

智能合约DSL中普遍存在一种合约场景特有的数据类型，即代表地址或身份的类型，用于唯一标识区块链上的账户。其中，Solidity的address地址类型，本质上是存储一个20字节的十六进制数，包含成员变量balance用于查询余额，成员函数transfer和send用于转账操作。除了地址类型外，Java具备了所有的通用数据类型，包括整型、布尔型等基本数据类型和数组、映射等引用数据类型。而其他语言在通用数据类型的支持上或多或少有些欠缺，比如Go不支持枚举类型，Solidity不支持浮点型。

### 内置变量和内置函数

内置变量和内置函数是指在全局命名空间中已经预设的一些特殊的变量和函数，主要用来提供关于区块链的信息或一些通用的工具函数。可以把这些变量和函数理解为语言层面的原生API。而通用编程语言在提供区块链信息的变量和函数方面相对欠缺。在智能合约DSL中，作为最主流的智能合约编程语言，Solidity的内置变量和函数最为完善。而通用编程语言在通用的工具函数方面相较于DSL更加丰富，即DSL中通用性的内置函数大部分在Java中都具备，比如编码及解码函数、基本数据类型操作的相关函数等，因此，在此只考虑智能合约场景下的内置函数。本文首先针对Solidity的内置变量或函数进行总结，再进行其他语言与Solidity的对比分析与补充，若为其子集，便不再赘述。最终归纳了四种语言的内置变量和五种语言的内置函数，分别如表3和表4所示。

对于内置变量，Solidity提供了一些关于区块和交易属性的内置变量，block包含当前区块的属性，msg包含调用函数时产生的消息，tx包含交易属性。Solidity为地址类型的变量提供了一些内置属性，包括该地址账户的余额等。Vyper补充了log对象作为日志记录器。Liquid支持调用self.env获取环境访问器，用于在合约代码中访问区块链执行上下文的某些信息。同理，Liquidity通过Current变量提供对当前对象属性的访问。

表3：

{\footnotesize
\begin{longtable}[H]{m{30pt} m{40pt} m{50pt} m{60pt} m{160pt}}
    \caption{语言内置变量对比} 
    \label{表-主流智能合约语言内置变量对比} \\
         \toprule    
         语言&内置变量&说明&方法或属性&说明\\
\midrule
Solidity&block&区块&chainid&区块链id\\
&&&number&区块号\\
&&&basefee&区块的基础费用\\
&&&gaslimit&区块gas限额\\
&&&coinbase&挖出区块的矿工地址\\
&&&difficulty&区块难度\\
&&&timestamp&区块时间戳\\
&msg&消息&data&完整的calldata\\
&&&sig&calldata的函数标识符\\
&&&sender&消息发送者\\
&&&value&随消息发送的以太币数量\\
&tx&交易&gasprice&交易的gas价格\\
&&&origin&交易发起者\\
&<address>&地址&balance&余额\\
&&&code&代码\\
&&&codehash&代码的哈希\\
\midrule
Vyper&log&日志记录器&/&/\\
\midrule
Liquid&self.env&环境访问器&get\_caller&合约调用者的账户地址\\
&&&get\_tx\_origin&调用链中，最开始的调用者的账户地址\\
&&&now&区块时间戳\\
&&&get\_address&合约自身的账户地址\\
&&&is\_contract&判断是否为合约账户\\
&&&emit&触发事件\\
\midrule
Liquidity&Current&当前对象&amount&总额\\
&&&balance&余额\\
&&&time&时间戳\\
&&&sender&发送者\\
        \bottomrule 
    \end{longtable} 
}

对于内置函数，Solidity提供了三个关于异常处理的内置函数，assert和require都具备类似校验的功能，revert则会直接终止交易运行。围绕转账功能，Solidity为地址类型的变量提供了四个内置函数。此外，Solidity还定义了两种特殊的回调函数，分别在接收以太币和调用不存在的函数时触发。Serpent支持三个类似于生命周期的内置函数，在合约函数调用的不同阶段执行。其他语言的内置函数所实现的功能基本类似。

表4：

{\footnotesize
\begin{longtable}[H]{m{30pt} m{50pt} m{270pt}}
    \caption{语言内置函数对比} 
    \label{表-主流智能合约语言内置函数对比} \\
         \toprule    
         语言&内置函数&说明\\
\midrule
Solidity&assert&若条件不满足，则会抛出异常，用于检查内部错误\\
&require&若条件不满足，则会抛出异常，用于检查由输入或外部引起的错误\\
&revert&终止交易运行并撤销状态更改\\
&transfer&发送以太币，失败时抛出异常\\
&send&发送以太币，失败时返回false\\
&call&发起低级CALL调用\\
&delegatecall&发起低级DELEGATECALL调用，失败时返回false\\
&selfdestruct&销毁合约，并把余额发送到指定地址\\
&receive&接收以太币时触发\\
&fallback&调用不存在的函数时触发\\
\midrule
DAML&ensure&确保条件成立\\
&assertMsg&确保条件成立，并接受自定义的错误消息\\
&getTime&获取时间\\
\midrule
Flint&send&将金额发送到指定地址\\
&fatalError&异常终止事务，并恢复任何状态更改\\
&assert&确保条件成立，否则会导致致命错误\\
\midrule
Serpent&init&在创建合约时执行\\
&shared&在init和用户函数之前执行\\
&any&在用户函数之前执行\\
\midrule
Liquid&require&若条件不满足，则终止运行，异常信息放入交易回执中返回\\
        \bottomrule 
    \end{longtable} 
}

## 实践习惯

对于第二个子任务，本小节从文件的维度分析了20个智能合约项目，作为后续对扩展智能合约领域语法进一步优化的实践依据。其中2个来自HF官方示例，10个来自GitHub，8个来自华为区块链合约仓库。

对于20个项目中包含账本读写操作的30个核心文件进行数据抽取与统计，观察后，对于开发者的实践习惯以及当前智能合约语法和API存在的不足之处，有如下发现和结论。在30个文件中：

- 有29个文件使用getStringState方法读取账本，11个文件使用putStringState方法写入账本，且后者是前者的一个子集。与此同时，一方面，只有5个文件没有手动进行byte[]和String类型的转换，这5个文件也是上述11个文件的一个子集；另一方面，余下19个使用getState或putState方法读写账本的文件，全部手动进行了byte[]和String类型的转换。换言之，凡使用byte[]类型作为账本读写操作的参数或返回值，无法避免需要额外的手动类型转换，而这完全可以在API设计时优化。
- 有20个文件进行了对象和Json字符串之间的转换，一部分借助了GSON、Jackson等第三方库，另一部分手动封装了转换方法。可见这一需求在账本读写场景下较为常见，而当前并没有便于操作的内置函数。
- 有23个文件对函数的入参进行了检查，包括入参长度是否符合预期、入参值是否为空等，这有助于智能合约的安全性。但是这部分处理逻辑冗长且重复率极高，值得在语言设计之初考虑优化。
- 有27个文件对账本读取函数的返回值进行了检查，可以分为前置处理和后置处理两类。前置处理即在读取账本操作前进行预读取，如果返回值不为空则进行后续真正的读取账本操作，否则抛出异常。后置处理即在读取账本操作后对返回值进行校验，若为空则抛出异常。同样，这部分处理逻辑也有待在语言设计层面进行优化。
- 有12个文件自定义了针对该文件内合约类的日志记录器，并记录了转账前后双方的余额等信息。实际上，这完全可以作为语言的内置变量。
- 用于导入第三方库的代码量占比平均为8.4%，换言之，在现有智能合约项目中，每100行代码就有约8至9行代码用于导入第三方库，这增加了开发者的工作量和记忆负担。