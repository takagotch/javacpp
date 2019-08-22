### javacpp
---
https://github.com/bytedeco/javacpp

```java
#include <string>

namespace NativeLibrary {
  class NativeClass {
    public:
      const std::string& get_property() { return property; }
      void set_property(const std::string& property) { this->property = property; }
      std::string property;
  };
}

import org.bytedeco.javacpp.*;
import org.bytedeco.javacpp.annotation.*;

@Platform(include="NativeLibrary.h")
@Namespace("NativeLibrary")
public class NativeLibrary {
  public static class NativeClass extends Pointer {
    static { Loader.load(); }
    public NativeClass() { allocate(); }
    private native void allocate();
    
    public native @StdString String get_property(); public native void set_property(String property);
    
    public native @StdString String property(); public native void property(String property);
  }
  
  public static void main(String[] args) {
    NativeClass l = new NativeClass();
    l.set_property("Hello World!");
    System.out.println(l.property());
  }
}


import org.bytedeco.javacpp.*;
import org.bytedeco.javacpp.annotation.*;

@Platform(include="<vector>")
public class VectorTest {
  
  @Name("std::vector<std::vector<void*> >")
  public static class PointerVectorVector extends Pointer {
    static { Loader.load(); }
    public PointerVectorVector() { allocate(); }
    public PointerVectorVector() { allocate(n); }
    public PointerVectorVector() { super(p); }
    private native void allocate();
    private native void allocate(long n);
    @Name("operator=")
    public native @ByRef PointerVecotVector put(@ByRef PointerVectorVector);
    
    @Name("operator[]")
    public native @StdVector PointerPointer get(long n);
    public native @StdVector PointerPointer at(long n);
    
    public native long size();
    public native @Cast("bool") boolean empty();
    public native void resize(long n);
    public native @Index long size(long i);
    public native @Index @Cast("bool") boolean empty(long i);
    public native @Index void resize(long i, long n);
    
    public native @Index Pointer get(long i, long j);
    public native void put(long i, long j, Pointer p);
  }
  
  public static void main(String[] args) {
    PointerVector v = new PointerVectorVector(13);
    v.resize(0, 42);
    Pointer p = new Pointer() { { address = 0xDEADBEEF; } };
    v.put(0, 0, p);
    
    PointerVectorVector v2 = new PointerVectorVector().put(v);
    Pointer p2 = v2.get(0).get();
    System.out.println(v2.size() + " " + v2.size(0) + " " + p2);
    
    v2.at(42);
  }
}


public class Processor {
  public static void process(java.nio.Buffer buffer, int size) {
    System.out.println("Processing in Java...");
  }
  
  public static void main(String[] args) {
    process(null, 0);
  }
}




```

```sh
java -jar javacpp.jar NativeLibrary.java -exec
java -jar javacpp.jar VectorTest.java -exec
```

```
```


