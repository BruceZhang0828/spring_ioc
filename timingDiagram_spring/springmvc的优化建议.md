## springmvc的优化建议

1.Controller如果能保持单例尽量保持单例,尽量使用单例.

2.@RequestParam 给具体的参数和url的参数进行一对一匹配

3.阅读源码的过程中,SpringMvc并没有对url和method对应关系进行缓存,建议自己对url和Method的关系进行缓存.

