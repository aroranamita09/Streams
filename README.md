# Streams
Streams Usage Interview Questions for java developers.

package com.demo.stream;

import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static java.util.stream.Collectors.*;

public class Main {
    static List<Employee> employeeList = new ArrayList<>();

    public static void main(String[] args) throws Exception {

        EmployeeFactory employeeFactory = new EmployeeFactory();
        employeeList = employeeFactory.getAllEmployee();
        //List all distinct project in non-ascending order.
        List<Integer> list = Arrays.asList(1, 2, 4, 5, 6);
        //Find average of a number
        double avg = list.stream().mapToInt(a -> a).average().getAsDouble();
        System.out.println("avg is" + avg);
        //Find number square average
        double avgSquare = list.stream().map(e -> e * e).filter(e -> e > 2).mapToInt(e -> e).average().getAsDouble();
        System.out.println("avg of squares is" + avgSquare);
        //There are multiple numbers in the list
        //Starting with 2 only
        List<Integer> listOfNumber = Arrays.asList(1, 2, 24, 52, 26,55,2,1);
        List<Integer> res = listOfNumber.stream().map(e -> String.valueOf(e)).
                filter(e -> e.startsWith("2")).
                map(Integer::valueOf).
                collect(Collectors.toList());
        System.out.println(res);
        //Find out duplicate numbers
        Set<Integer> dup =listOfNumber.stream().
                filter(e -> Collections.frequency(listOfNumber, e) > 1).
                collect(Collectors.toSet());
        System.out.println("Duplicate " + dup);
        //Use only set
        Set<Integer> dupNum =new HashSet<Integer>();
        Set<Integer> dup1 =listOfNumber.stream().filter(e-> !dupNum.add(e)).collect(Collectors.toSet());
        System.out.println("Duplicates" + dup1);
        int maxNum= listOfNumber.stream().max(Comparator.comparing(Integer :: valueOf)).get();
        System.out.println(maxNum);
        int minNum=listOfNumber.stream().min(Comparator.comparing(Integer::valueOf)).get();
        System.out.println(minNum);
        //how to sort the numbers
        System.out.println("Sorted ascending list" + list.stream().sorted().collect(Collectors.toList()));
        System.out.println("Sorted descending list" +list.stream().sorted(Collections.reverseOrder()).collect(Collectors.toList()));
        //how to get limit numbers
        //get first five numbers and sum of 5 numbers
        List<Integer> l= list.stream().limit(2).collect(Collectors.toList());
        int sum=list.stream().limit(2).reduce((p,q) -> p+q).get();
        System.out.println("SUm of reduced" + sum);
        //skip: ignore first five numbers and then print
        int sumskip=list.stream().skip(5).reduce((p,q)-> p+q).get();
        //second lowest and highest number
        int secondHighest= list.stream().sorted(Collections.reverseOrder()).distinct().skip(1).findFirst().get();
        System.out.println(secondHighest);
        //second lowest
        int secL =list.stream().sorted().distinct().skip(1).findFirst().get();

        Integer[] array = {1, 2, 3, 4, 1, 2, 3, 4, 5, 1, 2, 3};
        Arrays.stream(array).collect(Collectors.groupingBy(i->i,Collectors.counting()));
        String input= "Ram bought a cat and trained it to act like a dog and bull dog";
        String[] inputArray= input.split(" ");
        Stream<String> inputStream = Arrays.stream(inputArray);
        Map<String, Long> freq= inputStream.collect(Collectors.groupingBy(Function.identity(),Collectors.counting()));



        char[] charArray  ={'A', 'B', 'C', 'D', 'A', 'C', 'A', 'D'};
        int k =2;
        // A -> 3, C->2, D->2
        // O/P - A and C
        TreeMap<Character,Integer> map =new TreeMap<>();
        for( char c : charArray)
            map.put( c,map.getOrDefault(c,0)+1);

            
//        Arrays.stream(charArray).collect(Collectors.groupingBy(Function.identity(),Collectors.counting())).entrySet().stream()
//                .sorted(Map.Entry.<Character, Integer> comparingByValue())
//                .forEach((fruit)->System.out.println(fruit.getKey() + " -> " + fruit.getValue()));
//list.stream().reduce( (a,b)->a*b).get();

        
        Map<Integer,Employee> employe= employeeList.stream().collect(groupingBy(e->e.getSalary(),collectingAndThen(maxBy(Comparator.comparingInt(e-> e.getSalary())), Optional::get)));
        for(Map.Entry<Integer,Employee> e: employe.entrySet())
        System.out.println(e.getKey() + "Val" + e.getValue());

    }
}
