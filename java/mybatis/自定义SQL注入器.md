
自定义Mapper   extends BaseMapper<T>  增加方法

自己增加的方法  需要 extends AbstractMethod 方法

重写注入器   extends DefaultSqlInjector 重写 getMethodList 方法  记得 内部调用 super.getMethodList 方法

