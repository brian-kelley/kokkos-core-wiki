# `LOr`

Specific implementation of [ReducerConcept](ReducerConcept) performing logical `OR` operation

Header File: `Kokkos_Core.hpp`

Usage: 
```c++
T result;
parallel_reduce(N,Functor,LOr<T,S>(result));
```

## Synopsis 
```c++
template<class Scalar, class Space>
class LOr{
 public:
   typedef LOr reducer;
   typedef typename std::remove_cv<Scalar>::type value_type;
   typedef Kokkos::View<value_type, Space> result_view_type;
   
   KOKKOS_INLINE_FUNCTION
   void join(value_type& dest, const value_type& src)  const

   KOKKOS_INLINE_FUNCTION
   void join(volatile value_type& dest, const volatile value_type& src) const;

   KOKKOS_INLINE_FUNCTION
   void init( value_type& val)  const;

   KOKKOS_INLINE_FUNCTION
   value_type& reference() const;

   KOKKOS_INLINE_FUNCTION
   result_view_type view() const;

   KOKKOS_INLINE_FUNCTION
   LOr(value_type& value_);

   KOKKOS_INLINE_FUNCTION
   LOr(const result_view_type& value_);
};
```

## Public Class Members

### Typedefs
   
 * `reducer`: The self type.
 * `value_type`: The reduction scalar type.
 * `result_view_type`: A `Kokkos::View` referencing the reduction result 

### Constructors
 
 * ```c++
   LOr(value_type& result)
   ```
   Constructs a reducer which references a local variable as its result location.  
 
 * ```c++
   LOr(const result_view_type result)
   ```
   Constructs a reducer which references a specific view as its result location.

### Functions

 * ```c++
   void join(value_type& dest, const value_type& src)  const;
   ```
   Store logical `or` of `src` and `dest` into `dest`:  `dest = src || dest;`. 

 * ```c++
   void join(volatile value_type& dest, const volatile value_type& src) const;
   ```
   Store logical `or` of `src` and `dest` into `dest`:  `dest = src || dest;`. 

 * ```c++
   void init( value_type& val)  const;
   ```
   Initialize `val` using the Kokkos::reduction_identity<Scalar>::land() method.  The default implementation sets `val=0`.

 * ```c++
   value_type& reference() const;
   ```
   Returns a reference to the result provided in class constructor.

 * ```c++
   result_view_type view() const;
   ```
   Returns a view of the result place provided in class constructor.

### Additional Information
   * `LOr<T,S>::value_type` is non-const `T`
   * `LOr<T,S>::result_view_type` is `Kokkos::View<T,S,Kokkos::MemoryTraits<Kokkos::Unmanaged>>`.  Note that the S (memory space) must be the same as the space where the result resides.
   * Requires: `Scalar` has `operator =` and `operator ||` defined. `Kokkos::reduction_identity<Scalar>::lor()` is a valid expression. 
   * In order to use LOr with a custom type, a template specialization of `Kokkos::reduction_identity<CustomType>` must be defined.  See [Built-In Reducers with Custom Scalar Types](../../../ProgrammingGuide/Custom-Reductions-Built-In-Reducers-with-Custom-Scalar-Types) for details
