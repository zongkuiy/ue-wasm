1. \Engine\Source\Runtime\Core\Public\Math\TransformNonVectorized.h
#include "Math/ScalarRegister.h"
+ #include "Math/Quat.h"


2. try/Exception 注释掉

3. false_type -> ::std::false_Type

4. Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.53\upstream\emscripten\cache\sysroot/include/c++/v1\map
template <class _One>
struct __is_tree_value_type<_One> : __is_tree_value_type_imp<__remove_cvref_t<_One> > {};
---------->
template <class _One>
struct __is_tree_value_type<_One> : __is_tree_value_type_imp<::std::remove_cvref<_One> > {};
