# nuchange_assessment_UrlDatabase
The assessment repository of UrlDatabase project

## Usage Guide

To run the program, use the following command:
*'storeurl <url>': Store a URL with a unique short key.
* 'get <url>': Get the short key for a given URL.
* 'count <url>': Get the usage count for a given URL.
* 'list': Get a list of all URLs and their counts.
* 'exit': Terminate the program.

  ### Features Implemented
* Store URLs with unique short keys.
* The System generates the short key and updates count for a given URL.
* Get usage count for a given URL.
* List all URLs and counts ,using list command in the form of Json object.

  #### URL Database Program
  package com.nuchange_assignment;

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class UrlDatabase {
    private static Map<String, UrlInfo> urlMap = new HashMap<>();

    public static void main(String[] args) {
        System.out.println("Welcome to URL Database!");

        Scanner scanner = new Scanner(System.in);
        String command;

        do {
            System.out.print("Enter command: ");
            command = scanner.nextLine().trim();

            if (command.startsWith("storeurl")) {
                String url = command.split(" ", 2)[1].trim();
                storeUrl(url);
            } else if (command.startsWith("get")) {
                String url = command.split(" ", 2)[1].trim();
                String shortKey = getUrlShortKey(url);
                System.out.println("Short key for " + url + ": " + shortKey);
            } else if (command.startsWith("count")) {
                String url = command.split(" ", 2)[1].trim();
                int count = getUrlCount(url);
                System.out.println("Usage count for " + url + ": " + count);
            } else if (command.equals("list")) {
                System.out.println("URLs and Counts: " + getUrlList());
            }

        } while (!command.equals("exit"));

        System.out.println("Program terminated.");
    }

    private static void storeUrl(String url) {
        for (Map.Entry<String, UrlInfo> entry : urlMap.entrySet()) {
            if (entry.getValue().getUrl().equals(url)) {
                entry.getValue().incrementCount();
                System.out.println("URL count updated. Short key: " + entry.getKey());
                return;
            }
        }

        String shortKey = generateShortKey();
        urlMap.put(shortKey, new UrlInfo(url));
        System.out.println("URL stored with short key: " + shortKey);
    }

    private static String getUrlShortKey(String url) {
        for (Map.Entry<String, UrlInfo> entry : urlMap.entrySet()) {
            if (entry.getValue().getUrl().equals(url)) {
                return entry.getKey();
            }
        }
        return "URL not found";
    }

    private static int getUrlCount(String url) {
        for (UrlInfo urlInfo : urlMap.values()) {
            if (urlInfo.getUrl().equals(url)) {
                return urlInfo.getCount();
            }
        }
        return -1; // URL not found
    }

    private static String getUrlList() {
        StringBuilder result = new StringBuilder("{");
        for (Map.Entry<String, UrlInfo> entry : urlMap.entrySet()) {
            result.append("\"").append(entry.getValue().getUrl()).append("\": ").append(entry.getValue().getCount()).append(", ");
        }
        if (result.length() > 1) {
            result.delete(result.length() - 2, result.length());
        }
        result.append("}");
        return result.toString();
    }

    private static String generateShortKey() {
        
        return "key" + System.currentTimeMillis();
    }

    private static class UrlInfo {
        private String url;
        private int count;

        public UrlInfo(String url) {
            this.url = url;
            this.count = 1;
        }

        public String getUrl() {
            return url;
        }

        public int getCount() {
            return count;
        }

        public void incrementCount() {
            count++;
        }
    }
}

