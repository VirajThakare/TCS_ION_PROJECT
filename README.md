from textblob import TextBlob
import pandas as pd
import matplotlib.pyplot as plt

# Define a function to analyze sentiment using TextBlob
def analyze_sentiment(text):
    analysis = TextBlob(text)
    sentiment = analysis.sentiment

    if sentiment.polarity > 0.2:
        return "Positive"
    elif sentiment.polarity < -0.2:
        return "Negative"
    else:
        return "Neutral"

# Load your data from a CSV file (replace 'your_data.csv' with your data file)
df = pd.read_csv('C:\\Users\\Viraj\\Downloads\\comments\\sentiment-analysis.csv')

# Extract the comments from the combined column and analyze sentiment
sentiments = []

for row in df['Text, Sentiment, Source, Date/Time, User ID, Location, Confidence Score']:
    if isinstance(row, str):  # Check if the row is a string
        comment = row.split(',')[0].strip()  # Split the combined column and take the first part as the comment
        sentiment = analyze_sentiment(comment)
        sentiments.append(sentiment)
    else:
        sentiments.append("N/A")  # Handle non-string values

# Add the sentiment analysis results as a new column to the DataFrame
df['Sentiment'] = sentiments

# Calculate the total count of each sentiment
total_count = len(sentiments)
positive_count = sentiments.count("Positive")
negative_count = sentiments.count("Negative")
neutral_count = sentiments.count("Neutral")

# Calculate the average percentages
average_positive = (positive_count / total_count) * 100
average_negative = (negative_count / total_count) * 100
average_neutral = (neutral_count / total_count) * 100

# Save the updated DataFrame to a new CSV file (replace 'results.csv' with your desired output file)
df.to_csv('results.csv', index=False)

print("Sentiment analysis results saved to 'results.csv'.")
print("\nAverage Sentiment:")
print(f"Positive: {average_positive:.2f}%")
print(f"Negative: {average_negative:.2f}%")
print(f"Neutral: {average_neutral:.2f}%")

# Save the updated DataFrame to a new CSV file (replace 'results.csv' with your desired output file)
df.to_csv('results.csv', index=False)

# Generate a report and save it to a text file
report_filename = 'sentiment_report.txt'
with open(report_filename, 'w') as report_file:
    report_file.write("Sentiment Analysis Report\n")
    report_file.write("\nAverage Sentiment:\n")
    report_file.write(f"Positive: {average_positive:.2f}%\n")
    report_file.write(f"Negative: {average_negative:.2f}%\n")
    report_file.write(f"Neutral: {average_neutral:.2f}%\n")
    report_file.write("\nSentiment Counts:\n")
    report_file.write(f"Total: {total_count}\n")
    report_file.write(f"Positive: {positive_count}\n")
    report_file.write(f"Negative: {negative_count}\n")
    report_file.write(f"Neutral: {neutral_count}\n")
    report_file.write("\nSentiment Distribution:\n")
    report_file.write("Positive: {:.2f}%\n".format(average_positive))
    report_file.write("Negative: {:.2f}%\n".format(average_negative))
    report_file.write("Neutral: {:.2f}%\n".format(average_neutral))

print("Sentiment analysis results saved to 'results.csv'.")
print("Sentiment report saved to 'sentiment_report.txt'.")

# Example sentiment counts, replace these with your actual counts
sentiment_counts = {
    "Positive": average_positive,
    "Negative": average_negative,
    "Neutral": average_neutral
}

# Create a bar chart
plt.bar(sentiment_counts.keys(), sentiment_counts.values(), color=['green', 'red', 'gray'])
plt.xlabel('Sentiment')
plt.ylabel('Percentage')
plt.title('Sentiment Distribution')
plt.xticks(rotation=0)  # Rotate x-axis labels if needed

# Add labels to the bars
for sentiment, percentage in sentiment_counts.items():
    plt.text(sentiment, percentage, f'{percentage:.2f}%', ha='center', va='bottom')

# Display the bar chart
plt.show()
