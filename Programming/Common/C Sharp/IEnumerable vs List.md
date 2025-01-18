1.  IEnumerable - это базовый интерфейс для всех типов данных которые можно перечислить
IEnumerable
IEnumerator GetEnumerator()

IEnumerable<T>
IEnumerator<T> GetEnumerator()

IEnumerator позволяет итерироваться и делать перебор с помощью foreach


ICollection - дает возможность получать кол-во элементов + 
int Count {get;}
void Add();
void Remove(T item);
bool Contains(T item);
void Clear();


IList - имеет индексатор
Object this[int index]
void RemoveAt(int index);

IList<T>
T this[int index]
int IndexOf(T item)
void RemoveAt(int index);
void Insert(int index, T item)
Sort()

Если нужно работать с коллекцией на чтение то используйте IEnumerable<T> или
IReadOnlyList<T> если нужен индексатор а если нужно изменять содержимое списка то используйте IList


List это обертка над массивом
