```java

import java.util.*;

class Test {
	

   	// int[] a;

    public void m(int[] a){
    	a[0] = 11;
    	//System.out.println(a);
    }

    // public static void mi(int a){
    // 	a = 2;
    // }
    // public static void mr(int[] a){
    // 	a[0] = 2;
    // }
    // public static int instanceNumbers;

    // public static int getInstanceNumbers(){
    //     return instanceNumbers;
    // }

    // public Test(){
    //     instanceNumbers++;
    // }

    public int a, b;
    public Test(int a, int b){
        this.a = a;
        this.b = b;
    }


    public static void main(String args[]){

        Test t = new Test(1, 2);
        System.out.println(t.a +","+ t.b);
 		//Test t = new Test();
 		//int[] a = { 1, 2, 3 };

 		//t.m(a);

 		// t.a = { 2, 3, 4 };
 		//System.out.print(a[0]);
 		// t.m(t.a);

 		// System.out.println(a[0]);
    	
    	// String[] a = { "string1", "string2", "string3" };

    	// String[] b = a;

    	// System.out.println(b[0]);

    	// a[0] = "string11";

    	// System.out.println(b[0]);


 		// // int a = 1;
 		// // int b = 1;
 		// // System.out.print(a == b);
    	// int a = 1;
    	// int[] aa = { 1, 2, 3 };

    	// Test.mi(a);
    	// Test.mr(aa);

    	// System.out.print(a);
    	// System.out.print(aa[0]);

    	// int[] a = { 1, 2, "three" };

  		// int a[] = new int[3];
  		// a[2] = 3;

  		// for(int i=0; i<a.length; i++){
  		// 	System.out.println(a[i]);
  		// }
    	// List<T> list = new ArrayList<T>();
    	// list.add(1);
    	// list.add(2);
    	// list.set(3, 3);
    	// System.out.println(list.get(0));
    	// System.out.println(list.get(1));
    	// System.out.println(list.size());

    	// List<Integer> list = new ArrayList<Integer>();
    	// list.add(1);
    	// list.add(1);
    	// list.add(2);
    	// System.out.println(list);

    	// Set<Integer> set = new LinkedHashSet<Integer>();
    	// set.add(1);
    	// set.add(1);
    	// set.get(0);
    	// set.add(2);
    	// System.out.println(set);

        // String name = "name";
        // name = "rename";
        // Map<String, String> language = new HashMap<String, String>();

        // language.put(name, "test");
        // name = "NAME";

        // System.out.println(language.get(name));



  		
    }
}
```

```ruby
#p '𥊍'.length

# char = 'a'
# string = 'string'

# puts char
# puts string

# str = "string srt"
# equalStr = str
# puts equalStr
# equalStr = "string equalStr"
# puts str
# puts equalStr

# a = [1, 2, 3]

# b = a
# puts b

# a = ['one', 'two', 'three']

# puts b

# a = [ "A", "B", "C"]
# s = "Array a contains: "
# a.each do |i|
# 	s += i	
# 	puts "address of s is #{s.object_id}"
# end
# puts s

# require 'stringio'
# a = ["a", "b", "c"]
# s = StringIO.new
# s << "Array a contains: "
# a.each do |i|
# 	s << i
# 	puts "address of s is #{s.object_id}"
# end
# puts s.string


# :string = "String"

# string_a = "string"
# string_b = "string"

# symbol_a = :string
# symbol_b = :string

# puts symbol_b

# puts string_a.object_id 
# puts string_b.object_id
# puts symbol_a.object_id
# puts symbol_b.object_id

# a = [1, 2, "three"];
# puts a.length
# a[3] = 4
# puts a.length
# a[6] = 7
# puts a[5] == nil
# puts nil
# puts a.length
# require 'benchmark/ips'

# STRING_HASH = { "foo" => "bar" }
# SYMBOL_HASH = { :foo => "bar"  }

# Benchmark.ips do |x|
#   x.report("string") { STRING_HASH["foo"] }
#   x.report("symbol") { SYMBOL_HASH[:foo]  }
# end

# @a = :name
# l = { a=> "ruby" }
# @a = :rename
# puts l
# puts l[a]

# class Car
# 	@@instance_numbers = 0
# 	def initialize()
#     	@@instance_numbers += 1
#   	end

#   	def self.get_instance_numbers
#   		@@instance_numbers
#   	end
# end

# c = Car.new
# p Car.get_instance_numbers

# lambda{ |x| puts x + 1 }.call(2)
# Proc.new { |x| puts x + 1 }.call(2)

# a = lambda { |x| puts x*2 }
[1, 2, 3].each(&lambda { |x| puts x*2 })


# lam.call()

# class Demo
# 	attr_accessor :a, :b
# 	def initialize(a, b)	
# 		@a = a
# 		@b = b
# 	end
# end
# d = Demo.new(1, 2)
# puts d.a, d.b

# class Person
# end

# Person.class_eval do
#   def say_hello
#    "Hello!"
#   end
# end

# jimmy = Person.new
# jimmy.say_hello # "Hello!"
```

