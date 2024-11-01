
1. Eden区空间不足会触发Young GC
    
2. 老年代空间不足会触发Full GC
    
3. 方法区空间不足会触发 Full GC
    
4. 手动调用 System.gc() 会触发 Full GC
    
5. 老年代空间担保失败会触发 Full GC