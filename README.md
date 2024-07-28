import java.util.Scanner;
import java.util.Stack;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        int N = scanner.nextInt();
        int[][] matrix = new int[N][N];
        
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                matrix[i][j] = scanner.nextInt();
            }
        }
        
        int celebrity = findCelebrity(matrix, N);
        if (celebrity == -1) {
            System.out.println("No Celebrity");
        } else {
            System.out.println(celebrity);
        }
        
        scanner.close();
    }
    
    private static int findCelebrity(int[][] matrix, int N) {
        Stack<Integer> stack = new Stack<>();
        
        // Step 1: Push all people to the stack
        for (int i = 0; i < N; i++) {
            stack.push(i);
        }
        
        // Step 2: Get two people and compare them
        while (stack.size() > 1) {
            int a = stack.pop();
            int b = stack.pop();
            
            if (knows(matrix, a, b)) {
                // a knows b, so a is not a celebrity
                stack.push(b);
            } else {
                // a does not know b, so b is not a celebrity
                stack.push(a);
            }
        }
        
        // Step 3: Potential celebrity
        int candidate = stack.pop();
        
        // Step 4: Verify the candidate
        for (int i = 0; i < N; i++) {
            if (i != candidate) {
                if (knows(matrix, candidate, i) || !knows(matrix, i, candidate)) {
                    return -1;
                }
            }
        }
        
        return candidate;
    }
    
    private static boolean knows(int[][] matrix, int a, int b) {
        return matrix[a][b] == 1;
    }
}
