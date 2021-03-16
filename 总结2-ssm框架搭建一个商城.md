# SSM框架实现购物商城

## 2. UserService

###  2.1 ServerResponse服务器统一响应数据格式



```java
@JsonSerialize(include =  JsonSerialize.Inclusion.NON_NULL)
//保证序列化json的时候,如果是null的对象,key也会消失
public class ServerResponse<T> implements Serializable {

    private int status;
    private String msg;
    private T data;

    private ServerResponse(int status){
        this.status = status;
    }
    private ServerResponse(int status,T data){
        this.status = status;
        this.data = data;
    }

    private ServerResponse(int status,String msg,T data){
        this.status = status;
        this.msg = msg;
        this.data = data;
    }

    private ServerResponse(int status,String msg){
        this.status = status;
        this.msg = msg;
    }

    @JsonIgnore
    //使之不在json序列化结果当中
    public boolean isSuccess(){
        return this.status == ResponseCode.SUCCESS.getCode();
    }

    public int getStatus(){
        return status;
    }
    public T getData(){
        return data;
    }
    public String getMsg(){
        return msg;
    }


    public static <T> ServerResponse<T> createBySuccess(){
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode());
    }

    public static <T> ServerResponse<T> createBySuccessMessage(String msg){
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode(),msg);
    }

    public static <T> ServerResponse<T> createBySuccess(T data){
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode(),data);
    }

    public static <T> ServerResponse<T> createBySuccess(String msg,T data){
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode(),msg,data);
    }


    public static <T> ServerResponse<T> createByError(){
        return new ServerResponse<T>(ResponseCode.ERROR.getCode(),ResponseCode.ERROR.getDesc());
    }


    public static <T> ServerResponse<T> createByErrorMessage(String errorMessage){
        return new ServerResponse<T>(ResponseCode.ERROR.getCode(),errorMessage);
    }

    public static <T> ServerResponse<T> createByErrorCodeMessage(int errorCode,String errorMessage){
        return new ServerResponse<T>(errorCode,errorMessage);
    }
}
```

`1. 为什么使用包装类， 因为好处就是，如果我不需要向前端返回东西，但是我又不能返回一个void，因为前端不知道我处理好事情没，这时候你就必须返回一些东西。`

`2. 包装类ServerResponse使用了泛型public class ServerResponse<T> implements Serializable `

`3. @JsonSerialize(include =  JsonSerialize.Inclusion.NON_NULL) 表示对结果序列化,Null不用序列化`

`4. 泛型的方法调用方式  public <T> ServerResponse<T> createBySuccesss(T msg) 第一个<T>: 指明这个方法是这是一个泛型方法，持有一个泛型T  第二个 ServerResponse<T>: 这个方法返回一个泛型ServerResponse<T> 类型对象 createBySuccesss  ： 方法名  T msg: 一个泛型的参数`

### 2.2 userServiceImpl

```java

```

