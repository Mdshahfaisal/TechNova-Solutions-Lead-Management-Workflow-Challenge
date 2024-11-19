TechNova-Solutions-Lead-Management-Workflow-Challenge

1. Lead Scoring System

java
import java.util.HashMap;
import java.util.Map;

public class LScore {

    public static int calculateLeadScore(String companySize, String budget, String industry, String urgency) {
        Map<String, Integer> companySizeScores = Map.of(
            "1-50 employees", 10,
            "51-200 employees", 20,
            "201-1000 employees", 30,
            "1000+ employees", 40
        );

        Map<String, Integer> budgetScores = Map.of(
            "Less than $10,000", 10,
            "$10,000-$50,000", 20,
            "$50,001-$100,000", 30,
            "More than $100,000", 40
        );

        Map<String, Integer> industryScores = Map.of(
            "Technology", 30,
            "Finance", 25,
            "Healthcare", 20,
            "Retail", 15,
            "Other", 10
        );

        Map<String, Integer> urgencyScores = Map.of(
            "Immediate (within 1 month)", 40,
            "Short-term (1-3 months)", 30,
            "Medium-term (3-6 months)", 20,
            "Long-term (6+ months)", 10
        );

        return companySizeScores.getOrDefault(companySize, 0) +
               budgetScores.getOrDefault(budget, 0) +
               industryScores.getOrDefault(industry, 0) +
               urgencyScores.getOrDefault(urgency, 0);
    }

    public static void main(String[] args) {
        String companySize = "51-200 employees";
        String budget = "$10,000-$50,000";
        String industry = "Technology";
        String urgency = "Immediate (within 1 month)";

        int leadScore = calculateLeadScore(companySize, budget, industry, urgency);
        System.out.println("Lead Score: " + leadScore);
    }
}


2. Handling Edge Cases


public static boolean isLeadDataComplete(String companySize, String budget, String urgency) {
    return companySize != null && !companySize.isEmpty() &&
           budget != null && !budget.isEmpty() &&
           urgency != null && !urgency.isEmpty();
}

Identify High-Value Leads


public static boolean isHighValueLead(int leadScore, String budget) {
    return leadScore > 70 && "More than $100,000".equals(budget);
}

import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public static String convertToSalesTeamTimeZone(String leadTimeZone, String salesTeamTimeZone, String dateTime) {
    ZonedDateTime leadDateTime = ZonedDateTime.parse(dateTime, DateTimeFormatter.ISO_DATE_TIME);
    ZonedDateTime salesDateTime = leadDateTime.withZoneSameInstant(ZoneId.of(salesTeamTimeZone));
    return salesDateTime.format(DateTimeFormatter.ISO_DATE_TIME);
}

3. Advanced Implementations


public static String assignSalesRep(String lastAssignedRep, String[] salesReps) {
    int currentIndex = -1;

    for (int i = 0; i < salesReps.length; i++) {
        if (salesReps[i].equals(lastAssignedRep)) {
            currentIndex = i;
            break;
        }
    }

    int nextIndex = (currentIndex + 1) % salesReps.length;
    return salesReps[nextIndex];
}

public static void main(String[] args) {
    String[] salesReps = {"Rep1", "Rep2", "Rep3", "Rep4"};
    String lastAssignedRep = "Rep3";

    String nextRep = assignSalesRep(lastAssignedRep, salesReps);
    System.out.println("Next Sales Rep: " + nextRep);
}


Keyword Extraction for Comments 


import java.util.Arrays;
import java.util.List;

public static List<String> extractKeywords(String comments) {
    List<String> keywords = Arrays.asList("urgent", "budget", "healthcare", "technology");
    List<String> extracted = new ArrayList<>();

    for (String keyword : keywords) {
        if (comments.toLowerCase().contains(keyword)) {
            extracted.add(keyword);
        }
    }

    return extracted;
}



import com.google.api.services.calendar.Calendar;
import com.google.api.services.calendar.model.Event;
import com.google.api.services.calendar.model.EventDateTime;

import java.time.ZonedDateTime;

public static void scheduleFollowUp(Calendar service, String salesRepEmail, String summary, String description, String startDateTime, String endDateTime) throws Exception {
    Event event = new Event()
        .setSummary(summary)
        .setDescription(description)
        .setAttendees(Collections.singletonList(new Event.Attendee().setEmail(salesRepEmail)));

    EventDateTime start = new EventDateTime().setDateTime(startDateTime);
    EventDateTime end = new EventDateTime().setDateTime(endDateTime);

    event.setStart(start).setEnd(end);

    service.events().insert("primary", event).execute();
}
