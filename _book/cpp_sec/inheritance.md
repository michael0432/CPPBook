# Class Inheritance
###### tags = `OOP`

## 繼承(Inheritance)

繼承可以讓child class從parent class繼承其變數及方法，不必再重新寫一個一樣的變數或方法。

```cpp=1
class Animal{
private:
    string name;
public:
    Animal(string n){
        name = n;
    }
    string getName(){
        return name;
    }
};

class Bird: public Animal{
private:
    string category;
public:
	Bird(string s, string c):Animal(s){
		category = c;
	}
    string getCategory(){
        return category;
	}
};

class LittleBird: public Bird{
private:
	string color;
public:
	LittleBird(string col, string s, string c):Bird(s,c){
		color = col;
	}
	string getColor(){
		return color;
	}
};

int main(){
	LittleBird b("red","123","456");
	cout << b.getName() << b.getCategory() << b.getColor();
}
```

C++中的繼承分為三種：public, protected, private

| Access | public | protected | private |
| -------- | -------- | -------- | -------- |
| Same class     | yes     | yes     |  yes     |
| Child class | yes | yes | no |
| Outside class | yes | no | no |



