# 직렬화

<hr>

## 직렬화와 역직렬환
 * 직렬화 
   * 자바 시스템 내부에서 사용되는 Object 또는 Data를 외부의 자바 시스템에서도 사용할 수 있도록 byte 형태로 데이터를 변환하는 기술.
   * JVM(Java Virtual Machine 이하 JVM)의 메모리에 상주(힙 또는 스택)되어 있는 객체 데이터를 바이트 형태로 변환하는 기술
   * 인스턴스 값을 스트림으로 만드는 것
   * ObjectInputStream, ObjectOutPutStream을 사용
 * 역직렬화
   * byte로 변환된 Data를 원래대로 object나 Data로 변환하는 기술
   * 직렬화된 바이트 형태의 데이터를 객체로 변환하여 JVM으로 상주시키는 형태

## 직렬화화기
```java
class Person implements Serializable{
    private static final long serialVersionUID = -150325402544036183L;
    String name;
    transient String job;

    public Person(){}
    public Person(String name, String job){
        this.name =name;
        this.job = job;
       ...
       
    public staic void main(String[] args){ 
          Person person1 = new Person();
          ...
          
          #직렬화
       try(FileOutputStream fos = new FileOutputStream("serial.out");
           ObjectOutputStream oos = new ObjectOutputStream(fos)){
          oos.writeObject(person1);
          oos.writeObject(person2);
       } catch (IOException e) {
          e.printStackTrace();
       }
       
       #역직렬화
          
          try(FileInputStream fis = new FileInputStream("serial.out");
              ObjectInputStream ois = new ObjectInputStream(fis)) {
             Person p1 = (Person)ois.readObject();
             Person p2 = (Person)ois.readObject();
             System.out.println(p1);
             System.out.println(p2);
          } catch(IOException e){
             e.printStackTrace();
          }                  
       
```

## trasient 예약어
* 직렬화 대상이 되는 클래스는 모든 인스턴스 변수가 직렬화되고 복원됨
* 특정 변수는 직렬화하고 싶지 않을 떄 사용
* 자료형의 기본 값으로 저장(객체인 경우 null)
```java
   String name;
   transient String job; 
```

## Externalizable 인터페이스

### 해당 클래스에 Externalizable 인터페이스 구현
```java
    @Override
    public void writeExternal(ObjectOutput out) throws IOException{
        out.writeUTF(name);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException{
        name=in.readUTF();
    }
    ...
public static void main(String[] args) throws IOException, ClassNotFoundException{
        Dog myDog= new Dog();
        ...
        
        FileOutputStream fos = new FileOutputStream("external.out");
        ObjectOutputStream oos = new ObjectOutputStream(fos);

        try(fos; oos){
        oos.writeObject(myDog);
        } catch (Exception e) {
        e.printStackTrace();
        }

        FileInputStream fis = new FileInputStream("external.out");
        ObjectInputStream ois = new ObjectInputStream(fis);

        Dog dog = (Dog)ois.readObject();
        System.out.println(dog);
        }

```
