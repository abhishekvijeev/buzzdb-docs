Assignment 4: Operators
=======================

In this assignment, you will implement physical operators using the iterator model for your database system. This will involve building various operator classes that perform fundamental database operations such as selection, projection, sorting, joins, aggregation, and set operations.

Description
-----------

Your task is to develop physical operators that enable your database to execute SQL queries efficiently. These operators form the core of query execution, handling tasks like filtering data, combining datasets, and computing aggregations.

You will implement the following operations:

- **Print**: Outputs tuples with attributes separated by commas and tuples separated by newlines.
- **Projection**: Generates tuples containing a subset of attributes.
- **Select**: Filters tuples based on a predicate involving relational operators (``==``, ``!=``, ``<``, ``<=``, ``>``, ``>=``). Predicates compare an attribute (``l``) to another attribute or constant (``r``).
- **Sort**: Sorts tuples based on specified attributes and sort directions (ascending or descending).
- **HashJoin**: Performs an inner equi-join between two inputs on a specified attribute.
- **HashAggregation**: Groups tuples and computes aggregates (e.g., ``SUM``, ``COUNT``) over the groups.
- **Union**: Computes the union of two inputs without duplicates (set semantics).
- **UnionAll**: Computes the union of two inputs including duplicates (bag semantics).
- **Intersect**: Retrieves common tuples from two inputs without duplicates (set semantics).
- **IntersectAll**: Retrieves common tuples from two inputs including duplicates (bag semantics).
- **Except**: Returns tuples in the first input not present in the second input without duplicates (set semantics).
- **ExceptAll**: Returns tuples in the first input not present in the second input including duplicates (bag semantics).

Additionally, you will extend the ``parseQuery`` and ``executeQuery`` methods to allow execution of SQL-like join queries using the ``JoinExecutor`` implemented earlier (refer to test 14 for details).

Implementation Details
----------------------

You will work with the provided C++ skeleton code, which includes the base ``Operator`` class and derived classes for each operator. Your implementation will involve:

1. **Field Comparison Logic**: Implement comparison operators for the ``Field`` class to support predicates in selection and other operations.

2. **Operator Classes**: Implement the ``open()``, ``next()``, and ``close()`` methods for each operator. Where applicable, implement the ``get_output()`` method to retrieve the current tuple.

   - ``open()``: Initialize the operator before execution.
   - ``next()``: Retrieve the next tuple. Return ``true`` if a tuple is available.
   - ``close()``: Release resources after execution.
   - ``get_output()``: Return the pointers to the current tuple's fields after a successful ``next()``.

You may add additional member functions and variables to support your implementation. Use the provided ``Scan`` and ``Select`` operators as references.

**Iterator Model**: Operators should interact using the iterator model, where each operator pulls data from its child operator(s).

Testing and Validation
----------------------

Your implementation should pass all provided test cases to ensure correctness. Focus on:

- Correctness of each operator's logic.
- Proper handling of edge cases.
- Memory management and resource cleanup.

Building the Code
-----------------

Compile your code using:

.. code-block:: bash

   g++ -fdiagnostics-color -std=c++17 -O3 -Wall -Werror -Wextra <file_name.cpp> -o <output_name.out>

Ensure your code compiles without warnings or errors, as warnings are treated as errors.

FAQs
----

**How do I implement comparison logic for the ``Field`` class?**

You need to overload the comparison operators (``==``, ``!=``, ``<``, ``<=``, ``>``, ``>=``) for the ``Field`` class to compare field values correctly. This is essential for the ``Select`` operator and any operation that relies on comparing attribute values.

**What is the iterator model used in this assignment?**

The iterator model is a way of processing data where each operator requests data from its child by calling ``next()``. If the child returns ``true``, the operator can then process the data. This allows for efficient, pipeline-style execution with minimal memory overhead.

**How should I handle sorting in the ``Sort`` operator?**

In the ``Sort`` operator's ``open()`` method, you can read all tuples from the child operator and store them in a data structure like a vector. Then, sort the vector based on the specified attributes and sort directions. In the ``next()`` method, return tuples from the sorted vector one at a time.

**What's the difference between set semantics and bag semantics?**

- **Set Semantics**: Duplicate tuples are not included in the result. Operations like ``Union``, ``Intersect``, and ``Except`` eliminate duplicates.
- **Bag Semantics**: Duplicates are included in the result. Operations like ``UnionAll``, ``IntersectAll``, and ``ExceptAll`` retain duplicates based on their occurrence in the inputs.

**How do I extend ``parseQuery`` and ``executeQuery`` for join queries?**

Modify these methods to parse join queries and construct an operator tree that includes the ``JoinExecutor``. You'll need to recognize join syntax and create appropriate operator instances to handle the join operation.

**Can I add additional helper methods or variables?**

Yes, you are encouraged to add helper methods and member variables as needed to support your implementation, as long as they adhere to the assignment's requirements.

**How do I handle multiple aggregates in ``HashAggregation``?**

You can use a data structure (e.g., a map) to group tuples based on the grouping attributes. Then, compute the aggregates for each group by iterating over the grouped data and calculating the required aggregate functions.

**Do I need to handle complex predicates in ``Select``?**

For this assignment, each predicate consists of a single relational operator between an attribute and another attribute or constant. You are not required to handle compound predicates involving logical operators like ``AND`` or ``OR``.

**What resources can I refer to for this assignment?**

- The skeleton code and provided operator implementations (``Scan``, ``Select``).
- Lecture notes or textbooks covering database operators and the iterator model.
- Online resources or documentation on database physical operator implementations.

**How does the ``JoinExecutor`` work in this context?**

The ``JoinExecutor`` performs a join operation between two inputs based on specified attributes. In this assignment, you will integrate it into your operator tree to handle join queries parsed from SQL-like statements.

---