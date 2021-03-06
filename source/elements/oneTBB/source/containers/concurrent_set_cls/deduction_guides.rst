.. SPDX-FileCopyrightText: 2019-2020 Intel Corporation
..
.. SPDX-License-Identifier: CC-BY-4.0

================
Deduction guides
================

Where possible, constructors of ``concurrent_set`` support class template argument
deduction (since C++17):

.. code:: cpp

    template <typename InputIterator,
              typename Compare = std::less<iterator_value_t<InputIterator>>,
              typename Allocator = tbb_allocator<iterator_value_t<InputIterator>>>
    concurrent_set( InputIterator, InputIterator, Compare = Compare(), Allocator = Allocator() )
    -> concurrent_set<iterator_value_t<InputIterator>,
                      Compare,
                      Allocator>;

    template <typename InputIterator,
              typename Allocator>
    concurrent_set( InputIterator, InputIterator, Allocator )
    -> concurrent_set<iterator_value_t<InputIterator>,
                      std::less<iterator_key_t<InputIterator>>,
                      Allocator>;

    template <typename T,
              typename Compare = std::less<T>,
              typename Allocator = tbb_allocator<T>>
    concurrent_set( std::initializer_list<T>, Compare = Compare(), Allocator = Allocator() )
    -> concurrent_set<T, Compare, Allocator>;

    template <typename T,
              typename Allocator>
    concurrent_set( std::initializer_list<T>, Allocator )
    -> concurrent_set<T, std::less<Key>, Allocator>;

Where the type alias ``iterator_value_t`` is defined as follows:

.. code:: cpp

    template <typename InputIterator>
    using iterator_value_t = typename std::iterator_traits<InputIterator>::value_type;

**Example**

.. code:: cpp

    #include <tbb/concurrent_set.h>
    #include <vector>

    int main() {
        std::vector<int> v;

        // Deduces cs1 as concurrent_set<int>
        tbb::concurrent_set cs1(v.begin(), v.end());

        // Deduces cs2 as concurrent_set<int>
        tbb::concurrent_set cs2({1, 2, 3});
    }
