## 对 phpunit 进行了哪些修改
  - 去掉了哪些文件夹
    - php-code-coverage
    - phpdocumentor-reflection-docblock
    - php-invoker
    - phpspec-prophecy
    - php-token-stream
    - phpunit-selenium
    - sebastian-global-state
    - symfony

  - namespace 相关调整
    - 去掉了 class 开始部分的 namespace SebastianBergmann/Diff, use SebastianBergmann/Diff
    - class 命名由 class Diff {} 调整为 class SebastianBergmann_Diff_Diff {}

  - Reflection 类调整
  - dataProvider调整
    - 调整前: 查看方法注释中@dataProvider methodName 标注
    - 调整后: bpc环境下会查看是否存在'dataProvider' + methodName的方法, 如存在,认为当前方法的data由'dataProvider' + methodName方法提供(methodName首字符要大写): 如testAdd方法, 则为dataProviderTestAdd
  - hook方法规则调整
    - 调整前: 查看方法注释中@before @beforeClass @after @afterClass 标注
    - 调整后: bpc环境下会查看是否存在 'beforeMethod' + methodName, 'beforeClassMethod' + methodName, 'afterMethod' + methodName, 'afterClassMethod' + methodName + methodName的方法(methodName首字符要大写)
  - group规则调整
    - 调整前: 查看class以及method方法注释中@group groupName 标注
    - 调整后: bpc环境下class如果有分组,需定义在类中的 static $classGroups 数组; method如果有分组则查看是否存在'groups' + methodName的方法(methodName首字符要大写), 如存在,需返回所在分组的数组return array('groupName1', 'groupName2');
  - depend规则调整
    - 调整前: 查看class以及method方法注释中@depends methodName 标注
    - 调整后: bpc环境下class如果有依赖,需定义在类中的 static $classDepends 数组; method如果有依赖则查看是否存在'depends' + methodName的方法(methodName首字符要大写), 如存在,需返回所依赖的数组return array('methodName1', 'methodName2');

  - Mock调整
    - 先用php跑一遍，获取到Mock的Class，写入到mockClassFile文件夹中，第二次用bpc跑的时候直接include写好的MockClass文件
