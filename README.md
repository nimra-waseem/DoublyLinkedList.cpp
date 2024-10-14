#include <iostream>
using namespace std;
template <typename T>
class Node
{
    T data;
    Node* next;
    Node* prev;
public:
    Node(T val)
    {
        data = val;
        next = nullptr;
        prev = nullptr;
    }
    template <typename U>
    friend class LinkedList;
};
template <typename T>
class LinkedList
{
    Node<T>* head;
public:
    LinkedList()
    {
        head = nullptr;
    }
    void insertAtHead(T value)
    {
        Node <T>* newNode = new Node<T>(value);
        newNode->next = head;
        if (head != nullptr)
        {
            head->prev = newNode;
        }
        head = newNode;
    }
    void insertAtTail(T val)
    {
        if (head == nullptr)
        {
            insertAtHead(val);
            return;
        }
        Node<T>* temp = head;
        Node<T>* newNode = new Node<T>(val);
        while (temp -> next != nullptr)
        {
            temp = temp->next;
        }
        temp->next = newNode;
        newNode->prev = temp;
    }
    bool deleteAtTail()
    {
        if (head == nullptr)
        {
            return false;
        }
        if (head->next == nullptr)
        {
            delete head;
            head = nullptr;
            return false;
        }
        Node <T>* temp = head;
        while (temp->next -> next != nullptr)
        {
            temp = temp->next;
        }
        Node <T>* toDelete = temp->next;
        temp->next = nullptr;
        delete toDelete;
        return true;
    }
    bool deleteAtHead()
    {
        if (head == nullptr)
        {
            return false;
        }
        Node<T>* toDelete = head;
        head = head->next;  
        if (head != nullptr)
        {
            head->prev = nullptr; 
        }
        delete toDelete;
        return true;
    } 
    bool insertAfter(T value, T key)
    {
        Node<T>* temp = head;
        while (temp != nullptr)
        {
            if (temp->data == key)
            {
                Node<T>* newNode = new Node<T>(value);
                newNode->next = temp->next;
                newNode->prev = temp;
                temp->next = newNode;
                if (newNode->next != nullptr)
                {
                    newNode->next->prev = newNode;
                }
                return true;
            }
            temp = temp->next;
        }

        return false; 
    }
    bool insertBefore(T value, T key)
    {
      
        if (head == nullptr)
        {
            return false;
        }
        if (head->data == key)
        {
            insertAtHead(value);
            return true;
        }

        Node<T>* temp = head;
        while (temp != nullptr && temp->data != key)
        {
            temp = temp->next;
        }

        if (temp == nullptr)
        {
            return false; 
        }
        Node<T>* newNode = new Node<T>(value);
        newNode->next = temp;
        newNode->prev = temp->prev;
        if (temp->prev != nullptr)
        {
            temp->prev->next = newNode;
        }
        temp->prev = newNode;

        return true;
    }
    bool deleteBefore(T key)
    {
        if (head == nullptr || head->next == nullptr)
            return false; 
        Node<T>* temp = head;
        Node<T>* prevNode = nullptr;
        if (head->data == key)
            return false;
        while (temp != nullptr && temp->data != key)
        {
            prevNode = temp;
            temp = temp->next;
        }
        if (temp == nullptr || prevNode == nullptr || prevNode->prev == nullptr)
            return false; 
        Node<T>* toDelete = prevNode->prev;
        if (toDelete->prev != nullptr)
        {
            toDelete->prev->next = prevNode;
        }
        prevNode->prev = toDelete->prev;
        delete toDelete;
        return true;
    }

    bool deleteAfter(T key)
    {
        Node<T>* temp = head;

        if (head == nullptr || head->next == nullptr)
            return false; 
        while (temp != nullptr && temp->data != key)
        {
            temp = temp->next;
        }
        if (temp == nullptr || temp->next == nullptr)
            return false; 
        Node<T>* toDelete = temp->next;
        temp->next = toDelete->next;
        if (toDelete->next != nullptr)
        {
            toDelete->next->prev = temp;
        }
        delete toDelete;
        return true;
    }

    int getLength()
    {
        int count = 0;
        Node<T>* temp = head;
        while (temp != nullptr)
        {
            count++;
            temp = temp->next;
        }
        return count;
    }
    Node<T>* search(T x)
    {
        Node<T>* temp = head;
        while (temp != nullptr)
        {
            if (temp->data == x)
                return temp;
            temp = temp->next;
        }
        return nullptr;
    }
    LinkedList(const LinkedList<T>& other)
    {
        head = nullptr;
        Node<T>* temp = other.head;
        while (temp != nullptr)
        {
            insertAtTail(temp->data);
            temp = temp->next;
        }
    }
    ~LinkedList()
    {
        Node<T>* current = head;
        Node<T>* nextNode;
        while (current != nullptr)
        {
            nextNode = current->next; 
            delete current;           
            current = nextNode;       
        }
    }
    void display()
    {
        Node<T>* temp = head;
        while (temp != nullptr)
        {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }
    void insertSorted(T val)
    {
        Node<T>* newNode = new Node<T>(val);
        if (head == nullptr) 
        {
            head = newNode;
            return;
        }
        if (head->data >= val)
        {
            newNode->next = head;
            head = newNode;
            return;
        }
        Node<T>* temp = head;
        Node<T>* prev = nullptr;
        while (temp != nullptr && temp->data < val)
        {
            prev = temp;
            temp = temp->next;
        }
        newNode->next = temp;
        prev->next = newNode;
    }
    

};
int main()
{
    LinkedList<int>obj;
    obj.insertAtHead(50);
    obj.insertAtHead(40);
    obj.insertAtHead(20);
    obj.display();
    obj.insertAtTail(90);
    obj.display();
    obj.deleteAtTail();
    obj.display();
    obj.insertAfter(90,45);
    obj.display();
    obj.insertSorted(47);
    obj.display();
}
