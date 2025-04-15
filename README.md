import java.io.File;
import java.io.FileNotFoundException;
import java.util.*;
import java.util.regex.*;

public class ProjectAnni {

    // TreeMap for storing the index: word → count
    private static final TreeMap<String, Integer> wordIndex = new TreeMap<>();

    // Pattern for extracting words (letters, numbers, apostrophes)
    private static final Pattern WORD_PATTERN = Pattern.compile("[\\p{L}\\p{N}']+");

    // Main method
    public static void main(String[] args) {
        Scanner inputScanner = new Scanner(System.in);
        System.out.print("What is the name of the text file? ");
        String filename = inputScanner.nextLine();

        try {
            Scanner fileScanner = new Scanner(new File(filename));
            buildIndex(fileScanner);
            showIndex();
        } catch (FileNotFoundException ex) {
            System.err.println(filename + " not found");
            System.exit(1);
        }
    }

    /** Reads each word in a data file and stores it in an index with count.
        post: Lowercase form of each word with its count is stored in the index.
        @param scan A Scanner object for reading the file
     */
       public static void buildIndex(Scanner scan) {
        while (scan.hasNextLine()) {
            String line = scan.nextLine().toLowerCase();

            // Clean smart quotes or non-ASCII characters
            line = line.replaceAll("[^\\x00-\\x7F]", " ");

            Matcher matcher = WORD_PATTERN.matcher(line);
            while (matcher.find()) {
                String word = matcher.group();
                wordIndex.put(word, wordIndex.getOrDefault(word, 0) + 1);
            }
        }
    }

    /** Displays the index, one word per line. */
    public static void showIndex() {
        for (Map.Entry<String, Integer> entry : wordIndex.entrySet()) {
            System.out.println(entry.getKey() + "=" + entry.getValue());
        }
    }
}
