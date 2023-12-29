# tuyen
ko co gi
import java.util.*;
import java.util.stream.Collectors;

class Test {
    private String cardCategory;
    private String cardHoldName;

    public Test withCardCategory(String category) {
        this.cardCategory = category;
        return this;
    }

    public Test withCardHoldName(String holdName) {
        this.cardHoldName = holdName;
        return this;
    }

    public String getCardCategory() {
        return cardCategory;
    }

    public String getCardHoldName() {
        return cardHoldName;
    }
}

public class Application2 {
    public static void main(String[] args) {
        List<Test> list = Arrays.asList(
                new Test().withCardCategory("HH").withCardHoldName("CRE"),
                new Test().withCardCategory("DD").withCardHoldName("DEB"),
                new Test().withCardCategory("DD").withCardHoldName("CRE"),
                new Test().withCardCategory("HH").withCardHoldName("DEB"),
                new Test().withCardCategory("HH2").withCardHoldName("DEB")
        );

        System.out.println("old: " + list);

        List<Test> list2 = list.stream().sorted(Application2::compare).collect(Collectors.toList());

        System.out.println("new: " + list2);
    }

    public static int compare(Test o1, Test o2) {
        List<String> priority1 = Arrays.asList("HH", "DD");
        int categoryComparison = compareByPriority(o1.getCardCategory(), o2.getCardCategory(), priority1);
        
        if (categoryComparison == 0) {
            return compareCardHold(o1, o2);
        } else {
            return categoryComparison;
        }
    }

    private static int compareByPriority(String value1, String value2, List<String> priority) {
        if (priority.contains(value1) && !priority.contains(value2)) {
            return -1;
        } else if (!priority.contains(value1) && priority.contains(value2)) {
            return 1;
        } else {
            return Integer.compare(priority.indexOf(value1), priority.indexOf(value2));
        }
    }

    public static int compareCardHold(Test o1, Test o2) {
        List<String> priority2 = Arrays.asList("CRE", "DEB");
        return compareByPriority(o1.getCardHoldName(), o2.getCardHoldName(), priority2);
    }
}
