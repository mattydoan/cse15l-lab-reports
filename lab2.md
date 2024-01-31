Lab Report 2
![Image](website.png)

class Handler implements URLHandler {
    private String chatHistory = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/add-message")) {
            String query = url.getQuery();
            if (query != null) {
                String[] queryParams = query.split("&");
                String message = "";
                String user = "";

                for (String param : queryParams) {
                    String[] keyValue = param.split("=");
                    if (keyValue[0].equals("s")) {
                        message = keyValue.length > 1 ? keyValue[1] : "";
                    } else if (keyValue[0].equals("user")) {
                        user = keyValue.length > 1 ? keyValue[1] : "";
                    }
                }

                if (!message.isEmpty() && !user.isEmpty()) {
                    chatHistory += user + ": " + message + "\n";
                }
                return chatHistory;
            } else {
                return "Invalid request. Please use /add-message?s=<message>&user=<username>";
            }
        } else {
            return "404 Not Found!";
        }
    }
}
