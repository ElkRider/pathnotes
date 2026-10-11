



代码项目可以参考这里的：https://github.com/iluwatar/java-design-patterns

1. build model

   ```java
   @Data
   public class Computer {
       // 必填参数
       private final String cpu;
       private final String ram;
       
       // 选填参数
       private final String storage;
       private final String gpu;
       private final boolean hasBluetooth;
   
       // 1. 私有化构造方法，强制只能通过 Builder 来创建对象 private
       private Computer(Builder builder) {
           this.cpu = builder.cpu;
           this.ram = builder.ram;
           this.storage = builder.storage;
           this.gpu = builder.gpu;
           this.hasBluetooth = builder.hasBluetooth;
       }
   
       // 2. 静态内部类 Builder
       public static class Builder {
           // 必填参数（可以在 Builder 的构造方法中强制要求）
           private final String cpu;
           private final String ram;
           
           // 选填参数（设置默认值）
           private String storage = "256GB SSD";
           private String gpu = "集成显卡";
           private boolean hasBluetooth = false;
   
           // Builder 的构造方法，接收必填参数
           public Builder(String cpu, String ram) {
               this.cpu = cpu;
               this.ram = ram;
           }
   
           // 3. 设置选填参数的方法，每次返回 Builder 自身以支持链式调用
           public Builder storage(String storage) {
               this.storage = storage;
               return this;
           }
   
           public Builder gpu(String gpu) {
               this.gpu = gpu;
               return this;
           }
   
           public Builder hasBluetooth(boolean hasBluetooth) {
               this.hasBluetooth = hasBluetooth;
               return this;
           }
   
           // 4. 最终的 build 方法，返回外部类对象
           public Computer build() {
               return new Computer(this);
           }
       }
   
   
       @Override
       public String toString() {
           return "Computer{cpu='" + cpu + "', ram='" + ram + "', storage='" + storage + "', gpu='" + gpu + "'}";
       }
   }
   
   
   
   public class Main {
       public static void main(String[] args) {
           // 像搭积木一样创建对象，代码语义极其清晰
           Computer gamingPC = new Computer.Builder("Intel i9", "32GB")
                   .storage("1TB SSD")
                   .gpu("RTX 4090")
                   .hasBluetooth(true)
                   .build();
   
           Computer officePC = new Computer.Builder("Intel i5", "16GB")
                   .build(); // 使用默认配置
   
           System.out.println(gamingPC);
           System.out.println(officePC);
       }
   }
   
   
   这里设计，一个对象的构造只能通过构造方法来进行创建，但是一旦有一个有参数的构造方法后，默认的无参构造方法就已经没办法使用了。同时这里通过静态内部类的this引用
       
       
       
       
   ```

   