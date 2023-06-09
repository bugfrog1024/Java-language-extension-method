# RQ3：智能合约平台移植方法

经过调研，本小节总结了三类实现智能合约平台移植的方法，并对比了各类方法的优劣势，如表1所示。后续将在第三类方法的基础上进一步优化，在Java语言扩展方法中内置向其他平台或语言合约移植的接口。

第一类方法由Westerkamp等人提出，其提供了一个工具箱，实现了代码检索、状态重构和生成可部署的字节码等迁移过程的所有必要步骤，支持以灵活和可验证的方式在不同区块链平台之间迁移智能合约的逻辑和状态，促进了兼容EVM的区块链之间的智能合约移植。此外，在任何区块链节点中都可以通过比较原始状态和新状态的Merkle树哈希来验证迁移的有效性。但是，这种方法迁移的目标平台具有局限性，只能在原平台和目标平台的区块链底层虚拟机或容器相同的场景下才能进行迁移。

第二类方法由Zafar等人提出，其表示若想在另一个区块链平台上运行Solidity智能合约，可以将EVM合并到目标区块链中。倘若该方法得以落地，则实现了类似于Java程序可以运行在所有安装了JVM的计算机上的效果，即真正意义上的一次编写，多处运行。但Zafar等人也仅仅在理论上提出了这种思路，并认为这种方法的迁移成本很高。

第三类方法是目前比较常见的方法，即建立概念映射，实现语言翻译器，提供向目标平台的智能合约语言进行源到源的转换。Sol2js代码翻译工具实现了将Solidity智能合约转化到HF下的JavaScript智能合约，其首先建立了以太坊网络到HF网络的概念映射，接着通过基于Antlr4的解析器，解析Solidity源代码并创建一棵JSON格式的抽象语法树，通过两次遍历执行，先后获取函数、状态变量等定义以及翻译这些语句。Yan等人也提出了一种基于抽象语法树的语言转换方法，支持将EOS智能合约源代码转换为功能等效的形式化语言，以进行自定义语法构建，并通过添加符号实现自定义翻译过程。Crosara等人建立了Solidity语言到Takamaka语言在关键字和数据类型方面的概念映射，包括对于整数溢出问题的等效应对措施，其中Takamaka是用于编写Hotmoka区块链智能合约的一个Java子集。Chen等人提出了一个SPESC翻译器，用于将SPESC规范语言编写的智能合约向Solidity智能合约进行代码转换。SPESC翻译器提供了一系列生成规则，并通过使用Xtext工具实现了转换程序架构。这类方法在技术可行性方面相较于前两类方法更高，且支持任意区块链平台之间的智能合约迁移。但是，语言转换后的有效性往往依赖于人工验证，上述工作为了保证生成合约的有效性和可靠性，均进行了样本评估实验。

表1：

{\footnotesize
\begin{longtable}[htb]{m{160pt} m{120pt} m{80pt}}
    \caption{智能合约移植方法总结} 
    \label{表-智能合约语言移植的方法总结} \\
         \toprule    
方法&优势&劣势\\
\hline
迁移智能合约的逻辑和状态\cite{ref20}&支持已部署合约的状态迁移；便于验证有效性&迁移的目标平台具有局限性\\
\hline
虚拟机合并至目标平台\cite{ref9}&一次编写多处运行&迁移成本很高\\
\hline
概念映射与语言翻译\cite{ref9}\cite{ref84}\cite{ref85}\cite{ref21}&可行性高；支持任意平台的迁移&有效性依赖于人工验证\\
        \bottomrule 
    \end{longtable} 
}

第三类方法是现阶段较为主流的智能合约移植方案，且其属于语言层面的工作，适合融入语言设计中，因此最适用于本文研究。对于一门语言自身来说，本文认为在此基础之上，可以通过定义更为全面的智能合约领域的通用化语法，使其具备类似于统一的语言标准，以此降低其在建立与目标平台或语言之间的概念映射过程中的成本，进而降低后续实现语言翻译的复杂度。