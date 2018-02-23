Le Meetup
============

Dev4FunParis est un [meetup](https://www.meetup.com/fr-FR/Dev4Fun-Paris/) mensuel qui chercher à rassembler les développeurs dans une ambiance conviviale. Le but est d'apprendre et d'échanger sur nos méthodes de développement tout en s'amusant. 

Nous utilisons souvent la plateforme Codingame pour animer nos soirées :-) !

Date du prochain meetup : `le 14 Mars`

[2018-01-17] Dev4Fun #30
----------------------

Le Dev4fun 30 a eu lieu chez [Adneom](https://www.adneom.com/fr)

### Programme de la soirée : 

#### La dernière croisade
Un croisé, Indiana fils du Baron de Codingame s’est perdu dans un labyrinthe , une belle récompense sera offerte à ceux qui le ramènerons à bon port . [Énoncé](https://www.codingame.com/training/medium/the-last-crusade-episode-1)

notre solution en java : 

```java
import java.util.*;
import java.io.*;
import java.math.*;
import java.awt.Point;

/**
 * Auto-generated code below aims at helping you parse
 * the standard input according to the problem statement.
 **/
class Player {

    private static int[][] parseLines(List<String> lines) {
        int[][] grid = new int[lines.size()][];
        for (int i = 0; i < grid.length; i++) {
            String[] values = lines.get(i).split(" ");
            grid[i] = new int[values.length];
            for (int j = 0; j < values.length; j++) {
                grid[i][j] = Integer.parseInt(values[j]);
            }
        }
        return grid;
    }

    interface Trajectory {
        Point nextCell(Point cell);
    }

    static class Down implements Trajectory{
        public Point nextCell(Point entry) {
            return new Point(entry.x,entry.y+1);
        }
    }

    private static class Left implements Trajectory {
        public Point nextCell(Point cell) {
            return new Point(cell.x-1,cell.y);
        }
    }

    private static class Right implements Trajectory {
        public Point nextCell(Point cell) {
            return new Point(cell.x+1,cell.y);
        }
    }

    static class Cell {

        private final Trajectory top;
        private final Trajectory right;
        private final Trajectory left;

        public Cell(Trajectory top, Trajectory right, Trajectory left) {
            this.top = top;
            this.right = right;
            this.left = left;
        }

        private Point fromTop(Point entry) {
            return top.nextCell(entry);
        }

        private Point fromRight(Point entry) {
            return right.nextCell(entry);
        }

        private Point fromLeft(Point entry) {
            return left.nextCell(entry);
        }

        public Point nextCell(Point entry, String direction) {
            if("TOP".equals(direction))
                return fromTop(entry);
            if("RIGHT".equals(direction))
                return fromRight(entry);
            if("LEFT".equals(direction))
                return fromLeft(entry);
            return null;
        }
    }

    static final Trajectory DOWN = new Down();
    static final Trajectory LEFT = new Left();
    static final Trajectory RIGHT = new Right();
    static final Trajectory NONE = null;
    static final Cell[] CELLS_TYPES = new Cell[] {
        new Cell(DOWN,DOWN,DOWN),
        new Cell(NONE,LEFT,RIGHT),
        new Cell(DOWN,NONE,NONE),
        new Cell(LEFT,DOWN,NONE),
        new Cell(RIGHT,NONE,DOWN),
        new Cell(NONE,LEFT,RIGHT),
        new Cell(DOWN,DOWN,NONE),
        new Cell(NONE,DOWN,DOWN),
        new Cell(DOWN,NONE,DOWN),
        new Cell(LEFT,NONE,NONE),
        new Cell(RIGHT,NONE,NONE),
        new Cell(NONE,DOWN,NONE),
        new Cell(NONE,NONE,DOWN)
    };


    public static void main(String args[]) {
        Scanner in = new Scanner(System.in);
        int W = in.nextInt(); // number of columns.
        int H = in.nextInt(); // number of rows.
        if (in.hasNextLine()) {
            in.nextLine();
        }
        List<String> lines = new ArrayList<String>();
        for (int i = 0; i < H; i++) {
            String LINE = in.nextLine(); // represents a line in the grid and contains W integers. Each integer represents one room of a given type.
            lines.add(LINE);
        }
        int EX = in.nextInt(); // the coordinate along the X axis of the exit (not useful for this first mission, but must be read).
        int[][] grid = parseLines(lines);
        // game loop
        while (true) {
            int XI = in.nextInt();
            int YI = in.nextInt();
            String POS = in.next();
            Point start = new Point(XI,YI);
            // Write an action using System.out.println()
            // To debug: System.err.println("Debug messages...");
            int type = grid[start.y][start.x];
            if(type != 0) {
                Point nextCell = CELLS_TYPES[type-1].nextCell(start, POS);
                System.out.println(String.format("%d %d",nextCell.x, nextCell.y));
            } 
            // One line containing the X Y coordinates of the room in which you believe Indy will be on the next turn.
            
        }
    }
}
```

#### Le jeu des piles
Dans une taverne proche d’Hackerrank, Alexa défit les aventuriers un peu naïfs avec un jeu, venez lui donner une bonne leçon en la défiant !
[Énoncé](https://www.hackerrank.com/challenges/game-of-two-stacks/problem)


notre début solution en java : 

```java
import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class GameOf2StackTest {

    public static int stackGame(int x, int[] a, int[] b) {
        int[] aMax = new int[a.length + 1];
        aMax[0] = 0;
        for (int i = 0; i < a.length; i++) {
            aMax[i + 1] = a[i] + aMax[i];
        }
        int[] bMax = new int[b.length + 1];
        bMax[0] = 0;
        for (int i = 0; i < b.length; i++) {
            bMax[i + 1] = b[i] + bMax[i];
        }
        int max = 0;
        for (int i = 0; i < aMax.length; i++) {
            if (aMax[i] < x) {
                for (int j = 0; j < bMax.length; j++) {
                    if (aMax[i] + bMax[j] < x) {
                        if (i + j > max) {
                            max = i + j;
                        }
                    } else {
                        break;
                    }

                }
            } else return max;
        }
        return max;
    }
    
    @Test
    public void stackGame_simple_game() {
        int[] a = new int[]{4, 2, 4, 6, 1};
        int[] b = new int[]{2, 1, 8, 5};
        int x = 10;
        assertEquals(4, stackGame(x, a, b));
    }

    @Test
    public void stackGame_vicious_game() {
        int[] a = new int[]{4, 1, 1, 1, 1};
        int[] b = new int[]{2, 3, 3, 5};
        int x = 8;
        assertEquals(4, stackGame(x, a, b));
    }

}
```

[2018-01-17] Dev4Fun #29
----------------------

Le Dev4fun 29 a eu lieu chez [Adneom](https://www.adneom.com/fr)

<p align="center"><img src="dev4fun_29/dev4fun29.jpg" width="500"></p>

### Programme de la soirée :

#### Pertes en bourse
Les marchands de codingame blâment les dieux du chaos qui manipulerai leur pertes en bourse, venez enquêter. [énoncé](https://www.codingame.com/training/medium/stock-exchange-losses)

notre solution en java : 

```java
import org.junit.Assert;
import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class LosesTest {

    @Test
    public void closeCompareTest() {
        Assert.assertArrayEquals(new int[]{-1, -3}, closeCompare(new int[]{3, 2, 4, 2, 1, 5}));
    }

    @Test
    public void maxLosesTest() {
        Assert.assertEquals(-3, maxLoses(new int[]{-1, -3}));
    }

    @Test
    public void acceptanceTest() {
        Assert.assertEquals(-3, computeMaxLoses(new int[]{3, 2, 4, 2, 1, 5}));
        Assert.assertEquals(0, computeMaxLoses(new int[]{1,2,4,4,5}));
        Assert.assertEquals(-4, computeMaxLoses(new int[]{5, 3, 4, 2, 3, 1}));
    }

    private int computeMaxLoses(int[] ints) {
        return maxLoses(closeCompare(ints));
    }

    private int maxLoses(int[] ints) {
        int maxLoses = 0;
        for (int loses : ints) {
            maxLoses = Math.min(maxLoses,loses);
        }
        return maxLoses;
    }

    private int[] closeCompare(int[] ints) {
        List<Integer> indexes= new ArrayList<>();
        indexes.add(0);
        int indexCurrentMax = 0;
        for (int i = 1; i < ints.length - 2; i++) {
            if(ints[i] > ints[indexCurrentMax]) {
                indexCurrentMax = i;
                indexes.add(i);
            }
        }
        indexes.add(ints.length-1);
        int[] result = new int[indexes.size()-1];
        for (int i = 0; i < indexes.size()-1 ; i++) {
            int min = ints[indexes.get(i)];
            for (int j = indexes.get(i); j <= indexes.get(i+1) ; j++) {
                min = Math.min(min,ints[j]);
            }
            result[i]= min - ints[indexes.get(i)];
        }
        return result;
    }
}
```

#### Cases connectées
En creusant dans les tunnels d’hackerrank les alchimistes on découverts de curieux mécanismes qui fonctionnent avec des tablettes, ils n’arrivent pas à les remettre en route et cherche des ingénieurs. [énoncé](https://www.hackerrank.com/challenges/connected-cell-in-a-grid/problem)

notre début solution en java : 

```java
import org.junit.Assert;
import org.junit.Test;

import java.util.*;

public class GridTest {

    @Test
    public void computeLinksTest() {
        Map<Integer,Set<Integer>> expectedLinks = new HashMap<>();
        expectedLinks.put(0, Collections.singleton(3));
        Assert.assertEquals(expectedLinks,
                computeLinks(new boolean[][]{
                        new boolean[]{true,false},
                        new boolean[]{false,true},
                })
        );
    }

    @Test
    public void mergeSetsTest() {
        Map<Integer,Set<Integer>> links = new HashMap<>();
        links.put(0,Set.of(1,5));
        links.put(1,Set.of(5,6));
        links.put(5,Set.of(6,10));
        links.put(6,Set.of(10));
        links.put(10,Set.of(11));
        Assert.assertEquals(6,groupMax(links));

    }

    private int groupMax(Map<Integer, Set<Integer>> links) {
        Iterator<Integer> keys = links.keySet().iterator();
        while (keys.hasNext()){
            List<Integer> elements = new ArrayList<>();
            Integer key = keys.next();
            elements.addAll(links.get(key));
            r(links,elements,0);

        }
        return 0;
    }

    private void r(Map<Integer,Set<Integer>> links, List<Integer> elements,int index){
        List<Integer> toAdd = new ArrayList<>();
        for (int k  = index; k < elements.size(); k++) {
            toAdd.addAll(links.get(elements.get(index)));
            //
        }

        if(!elements.containsAll(toAdd)) {
            elements.addAll(toAdd);
            r(links, elements, index + toAdd.size());
        }
    }


    @Test
    public void computeLinksTest2() {
        Map<Integer,Set<Integer>> expectedLinks = new HashMap<>();
        expectedLinks.put(0,Set.of(1,5));
        expectedLinks.put(1,Set.of(5,6));
        expectedLinks.put(5,Set.of(6,10));
        expectedLinks.put(6,Set.of(10));
        expectedLinks.put(10,Set.of(11));
        boolean[][] grid = new boolean[][]{
                new boolean[]{true,true,false,false},
                new boolean[]{false,true,true,false},
                new boolean[]{false,false,true,false},
                new boolean[]{true,false,false,true},
        };

        for (boolean[] line :
                grid) {
            System.out.println(Arrays.toString(line));
        }

        System.out.println(grid[0][1]);

        Assert.assertEquals(expectedLinks,
                computeLinks(grid)
        );
    }

    private Map<Integer, Set<Integer>> computeLinks(boolean[][] grid) {
        Map<Integer,Set<Integer>> links = new HashMap<>();

        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[i].length; j++) {
                if(grid[i][j]) {
                    addNeighbors(grid,i,j,links);
                }
            }
        }

        return links ;
    }

    private void addNeighbors(boolean[][] grid, int i, int j, Map<Integer, Set<Integer>> links) {
        int gridIndex = j + i*grid[i].length;
        Set<Integer> neighbors = new HashSet<>();
        for (int k = Math.max(i-1,0); k <= Math.min(grid.length-1,i+1) ; k++) {
            for (int l = Math.max(j-1,0); l <= Math.min(grid[k].length-1,j+1) ; l++) {
                System.out.println(String.format("%d,%d %b",k,l,grid[k][l]));
                if( !(k == i && l == j) && grid[k][l]) {
                    int n = l+(k*grid[k].length);
                    if(n > gridIndex)
                        neighbors.add(n);
                }
            }
        }
        if(!neighbors.isEmpty()) {
            links.put(gridIndex, neighbors);
        }
    }

}
```

[2017-12-13] Dev4Fun #28
------------------------

Le Dev4fun 28 a eu lieu chez [Adneom](https://www.adneom.com/fr)

<p align="center"><img src="dev4fun_28/dev4fun28-1.jpg" width="275">   <img src="dev4fun_28/dev4fun28-2.jpg" width="275"></p>


### Programme de la soirée : 

#### Suite de Conway
Sorciers du code, choisissez votre langage et vos compagnons pour affronter la « SUITE DE CONWAY » sur les terres de Codingame.
[énoncé](https://www.codingame.com/training/medium/conway-sequence)

notre solution en groovy : 

```groovy
input = new Scanner(System.in);

r = input.nextInt()
l = input.nextInt()

c = [[r],[1,r]]
(2..l-1).each {
    List list = c[it - 1]
    result = []
    i = 1
    while (i <= list.size()) {
        count = 1
        while (list[i - 1] == list[i]) {
            count++
            i++
        }
        result.add(count)
        result.add(list[i - 1])
        count = 1
        i++
    }
    c.add(result)
}
println(c[l-1].join(" "))
```

#### Hackerland radio transmitters
Dans les tunnels d’Hackerrank, la cité Hackerland à besoin d’ingénieurs pour rétablir leur système de communication.
[énoncé](https://www.hackerrank.com/challenges/hackerland-radio-transmitters/problem)

notre solution en groovy : 

```groovy
s= new Scanner(System.in);
int n = s.nextInt();
int portee = s.nextInt();
maisons=new TreeSet<>()
for(int x_i=0; x_i < n; x_i++){
    maisons.add(s.nextInt())
}

min = maisons.min()
max = maisons.max()
transmiters=[]
for(i = min; i <= max; i++) {
    if (maisons.contains(i)) {
        k = i + portee
        while (!maisons.contains(k)) {
            k--
        }
        transmiters.add(k)
        i = k + portee
    }
}

println(transmiters.size())
```
