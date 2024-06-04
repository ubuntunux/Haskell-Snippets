[https://mail.haskell.org/pipermail/glasgow-haskell-users/2014-December/025502.html](https://mail.haskell.org/pipermail/glasgow-haskell-users/2014-December/025502.html)

Keep variables live (when using interior pointers)

so touch is preserved through the CMM level, and then gets erased when doing final code gen.
Its meant to ensure on heap pointers remain reachable

the point of touch is to prevent premature GC, it actually gets erased at the CMM level i believe.
That is, it only makes sense to apply touch to lifted types on the heap!

아래의 코드에서 layoutCreateInfo 주목해보자.
let layoutCreateInfo으로 선언된 후 withPtr 함수에 바인딩 된후 더이상 사용되는 곳이 없다.
layoutCreateInfoPtr는 layoutCreateInfo의 주소를 담고있는 변수이지만 layoutCreateInfo의 reference count에 영향을 주지 않으므로  layoutCreateInfo는 GC에 의해서 언제든지 지워질수 있기 때문에 layoutCreateInfoPtr는 유효하지 않을 수있다. GC에 의해서 layoutCreateInfo가 사라지지 않도록 방지 하는것이 아랫쪽의 touch layoutCreateInfo 함수의 역활이다.

```
let layoutCreateInfo = createVk @VkDescriptorSetLayoutCreateInfo
let layoutCreateInfoPtr = unsafePtr layoutCreateInfo
vkCreateDescriptorSetLayout device layoutCreateInfoPtr VK_NULL descriptorSetLayoutPtr
touch layoutCreateInfo
```