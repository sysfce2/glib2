Title: Data Structures

# Data Structures

GLib includes a number of basic data sructures, such as linked lists, hash tables,
arrays, queues, etc.

## Arrays

GLib arrays ([struct@GLib.Array]) are similar to standard C arrays, except that they grow
automatically as elements are added.

Array elements can be of any size (though all elements of one array are the same size),
and the array can be automatically cleared to '0's and zero-terminated.

To create a new array use [method@Glib.Array.new].

To add elements to an array with a cost of O(n) at worst, use
[methodd@GLib.Array.append_val],
[method@GLib.Array.append_vals],
[method@GLib.Array.prepend_val],
[method@GLib.Array.prepend_vals],
[method@GLib.Array.insert_val] and
[method@GLib.Array.insert_vals].

To access an element of an array in O(1) (to read it or to write it),
use [method@GLib.Array.index].

To set the size of an array, use [method@GLib.Arrray.set_size].

To free an array, use [method@GLib.Array.unref] or [method@GLib.Array.free].

All the sort functions are internally calling a quick-sort (or similar)
function with an average cost of O(n log(n)) and a worst case cost of O(n^2).

Here is an example that stores integers in a [struct@GLib.Array]:

```c
GArray *array;
int i;

// We create a new array to store int values.
// We don't want it zero-terminated or cleared to 0's.
array = g_array_new (FALSE, FALSE, sizeof (int));

for (i = 0; i < 10000; i++)
  {
    g_array_append_val (array, i);
  }

for (i = 0; i < 10000; i++)
  {
    if (g_array_index (array, int, i) != i)
      g_print ("ERROR: got %d instead of %d\n",
               g_array_index (array, int, i), i);
  }

g_array_free (array, TRUE);
```

## Pointer Arrays

Pointer Arrays ([struct@GLib.PtrArray]) are similar to Arrays but are used
only for storing pointers.

If you remove elements from the array, elements at the end of the
array are moved into the space previously occupied by the removed
element. This means that you should not rely on the index of particular
elements remaining the same. You should also be careful when deleting
elements while iterating over the array.

To create a pointer array, use [method@GLib.PtrArray.new].

To add elements to a pointer array, use [method@GLib.PtrArray.add].

To remove elements from a pointer array, use
[method@GLib.PtrArray.remove],
[method@GLib.PtrArray.remove_index] or
[method@GLib.PtrArray.remove_index_fast].

To access an element of a pointer array, use [method@GLib.PtrArray.index].

To set the size of a pointer array, use [method@GLib.PtrArray.set_size].

To free a pointer array, use [method@GLib.PtrArray.unref] or [method@GLib.PtrArray.free].

An example using a [struct@Glib.PtrArray]:

```c
GPtrArray *array;
char *string1 = "one";
char *string2 = "two";
char *string3 = "three";

array = g_ptr_array_new ();
g_ptr_array_add (array, (gpointer) string1);
g_ptr_array_add (array, (gpointer) string2);
g_ptr_array_add (array, (gpointer) string3);

if (g_ptr_array_index (array, 0) != (gpointer) string1)
  g_print ("ERROR: got %p instead of %p\n",
           g_ptr_array_index (array, 0), string1);

g_ptr_array_free (array, TRUE);
```

## Byte Arrays

{struct@Glib.ByteArray] is a mutable array of bytes based on [struct@Glib.Array],
to provide arrays of bytes which grow automatically as elements are added.

To create a new `GByteArray` use [method@GLib.ByteArray.new].

To add elements to a `GByteArray`, use
[method@GLib.ByteArray.append] and [method@GLib.ByteArray.prepend].

To set the size of a `GByteArray`, use [method@GLib.ByteArray.set_size].

To free a `GByteArray`, use [method@Glib.ByteArray.unref] or [method@Glib.ByteArray.free].

An example for using a `GByteArray`:

```c
GByteArray *array;
int i;

array = g_byte_array_new ();
for (i = 0; i < 10000; i++)
  {
    g_byte_array_append (array, (guint8*) "abcd", 4);
  }

for (i = 0; i < 10000; i++)
  {
    g_assert (array->data[4*i] == 'a');
    g_assert (array->data[4*i+1] == 'b');
    g_assert (array->data[4*i+2] == 'c');
    g_assert (array->data[4*i+3] == 'd');
  }

g_byte_array_free (array, TRUE);
```

See [struct@Glib.Bytes] if you are interested in an immutable object representing a
sequence of bytes.

## Singly-linked Lists

The [struct@GLib.SList] structure and its associated functions provide a standard
singly-linked list data structure. The benefit of this data structure is to provide
insertion/deletion operations in O(1) complexity where access/search operations are
in O(n). The benefit of `GSList` over [struct@GLib.List] (doubly-linked list) is that
they are lighter in space as they only need to retain one pointer but it double the
cost of the worst case access/search operations.

Each element in the list contains a piece of data, together with a pointer which links
to the next element in the list. Using this pointer it is possible to move through the
list in one direction only (unlike the [double-linked lists][glib-Doubly-Linked-Lists],
which allow movement in both directions).

The data contained in each element can be either integer values, by
using one of the [Type Conversion Macros][glib-Type-Conversion-Macros],
or simply pointers to any type of data.

Note that most of the #GSList functions expect to be passed a pointer to the first element
in the list. The functions which insert elements return the new start of the list, which
may have changed.

There is no function to create a `GSList`. `NULL` is considered to be the empty list so you
simply set a `GSList*` to `NULL`.

To add elements, use [method@GLib.SList.append], [method@GLib.SList.prepend],
[method@GLib.SList.insert] and [method@GLib.SList.insert_sorted].

To remove elements, use [method@GLib.SList.remove].

To find elements in the list use [method@GLib.SList.last], [method@GLib.SList.next],
[method@GLib.SList.nth], [method@GLib.SList.nth_data], [method@GLib.SList.find] and
[method@GLib.SList.find_custom].

To find the index of an element use [method@GLib.SList.position] and [method@GLib.SList.index].

To call a function for each element in the list use [method@GLib.SList.foreach].

To free the entire list, use [method@GLib.SList.free].

## Doubly-linked Lists

The [struct@Glib.List[ structure and its associated functions provide a standard
doubly-linked list data structure. The benefit of this data-structure is to provide
insertion/deletion operations in O(1) complexity where access/search operations are in O(n).
The benefit of `GList` over [struct@Glib.SList] (singly-linked list) is that the worst case
on access/search operations is divided by two which comes at a cost in space as we need
to retain two pointers in place of one.

Each element in the list contains a piece of data, together with pointers which link to the
previous and next elements in the list. Using these pointers it is possible to move through
the list in both directions (unlike the singly-linked [GSList][glib-Singly-Linked-Lists],
which only allows movement through the list in the forward direction).

The doubly-linked list does not keep track of the number of items and does not keep track of
both the start and end of the list. If you want fast access to both the start and the end of
the list,  and/or the number of items in the list, use a [GQueue][glib-Double-ended-Queues] instead.

The data contained in each element can be either integer values, by using one of the
[Type Conversion Macros][glib-Type-Conversion-Macros], or simply pointers to any type of data.

Note that most of the `GList` functions expect to be passed a pointer to the first element in the list.
The functions which insert elements return the new start of the list, which may have changed.

There is no function to create a `GList`. `NULL` is considered to be a valid, empty list so you simply
set a `GList*` to `NULL` to initialize it.

To add elements, use [method@GLib.List.append], [method@GLib.List.prepend],
[method@GLib.List.insert] and [method@GLib.List.insert_sorted].

To visit all elements in the list, use a loop over the list:

```c
GList *l;
for (l = list; l != NULL; l = l->next)
  {
    // do something with l->data
  }
```

To call a function for each element in the list, use [method@GLib.List.foreach].

To loop over the list and modify it (e.g. remove a certain element) a while loop is more appropriate,
for example:

```c
GList *l = list;
while (l != NULL)
  {
    GList *next = l->next;
    if (should_be_removed (l))
      {
        // possibly free l->data
        list = g_list_delete_link (list, l);
      }
    l = next;
  }
```

To remove elements, use [method@GLib.List.remove[.

To navigate in a list, use [method@GLib.List.first], [method@GLib.List.last],
[method@GLib.List.next], [method@GLib.List.previous].

To find elements in the list use [method@GLib.List.nth], [method@GLib.List.nth_data],
[method@GLib.List.find] and [method@GLib.List.find_custom].

To find the index of an element use [method@GLib.List.position] and [method@GLib.List.index].

To free the entire list, use [method@GLib.List.free] or [method@GLib.List.free_full].
