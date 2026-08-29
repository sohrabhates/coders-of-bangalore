Coders of Bangalore

Project Overview

This project processes raw Instagram follower data collected for the Coders of Bangalore task.

The goal is to read the collected Instagram data from a text file, parse each profile into structured Python dictionaries, and answer the following questions:

Who has the maximum number of posts?

Who has the maximum number of followers?

Who follows the maximum number of people?

How many different categories are present?

How many people are there in each category?

Files

coders-of-bangalore (1).ipynb — Jupyter Notebook containing the Python data-processing logic.

finaldata.txt — Input text file containing the final Instagram profile data.

The notebook originally used initialdata.txt. For the final version, the input file should be finaldata.txt.

Data Structure

Each Instagram profile generally follows this structure:

username
posts
followers
following
name
type_of_page
bio / additional information

Example:

intaglobal
1,946 posts
6,851 followers
262 following
INTA
Nonprofit organization
The association of ™ professionals.

Some profiles may not contain all optional fields, so the parser handles missing name or type_of_page values.

Processing Workflow

The notebook follows this general workflow:

finaldata.txt
     ↓
read file
     ↓
split data into chunks
     ↓
parse each profile
     ↓
create all_chunks
     ↓
clean numerical values
     ↓
answer analytical questions

1. Read the data

with open("finaldata.txt", encoding="utf-8") as f:
    data = f.read()

2. Split the data into profiles

Profiles are separated by blank lines:

chunks = data.split("\n\n")
chunks = [c for c in chunks if len(c) > 3]

3. Parse each profile

The parser extracts the main fields:

def parse_chunk(chunk):
    chunk = chunk.strip()
    lines = chunk.split("\n")

    username = lines[0]
    posts = lines[1]
    followers = lines[2]
    following = lines[3]

    name = lines[4] if len(lines) > 4 else ""
    type_of_page = lines[5] if len(lines) > 5 else ""

    return {
        "username": username,
        "posts": posts,
        "followers": followers,
        "following": following,
        "name": name,
        "type_of_page": type_of_page
    }

4. Create the parsed dataset

all_chunks = []

for chunk in chunks:
    parsed_chunk = parse_chunk(chunk)
    all_chunks.append(parsed_chunk)

Number Conversion

The raw data contains values such as:

1,946 posts

6,851 followers

681K followers

45K followers

The parse_number() function converts these values into numbers that Python can compare:

def parse_number(value):
    value = value.strip().replace(",", "").split()[0]

    if value.endswith("K"):
        return int(float(value[:-1]) * 1_000)
    elif value.endswith("M"):
        return int(float(value[:-1]) * 1_000_000)
    else:
        return int(value)

For example:

6,851 followers → 6851
681K followers  → 681000
45K followers   → 45000

Finding Maximum Posts

max_posts = 0

for chunk in all_chunks:
    posts = int(chunk["posts"].replace(",", "").split()[0])

    if max_posts < posts:
        max_posts = posts
        chunk_with_max_post = chunk

print(chunk_with_max_post)

Finding Maximum Followers

max_followers = 0

for chunk in all_chunks:
    followers = parse_number(chunk["followers"])

    if max_followers < followers:
        max_followers = followers
        chunk_with_max_followers = chunk

print(chunk_with_max_followers)

Finding Maximum Following

max_following = 0

for chunk in all_chunks:
    following = parse_number(chunk["following"])

    if max_following < following:
        max_following = following
        chunk_with_max_following = chunk

print(chunk_with_max_following)

Finding the Number of Categories

A Python set is used to store unique categories:

categories = set()

for chunk in all_chunks:
    categories.add(chunk["type_of_page"])

print(categories)
print(len(categories))

The set automatically removes duplicate category names.

Important Notes

Do not use max as a variable name

Avoid:

max = 0

because max is already a built-in Python function.

Prefer:

max_posts = 0
max_followers = 0
max_following = 0

Run cells in order

The notebook depends on variables created by previous cells. The recommended execution order is:

Read finaldata.txt

Create chunks

Define parse_chunk()

Create all_chunks

Define parse_number()

Run the analysis cells

If the parser is changed, rerun the parser cell and the cells that depend on it.

Requirements

Python 3

Jupyter Notebook / JupyterLab

No external Python packages are required for the basic processing.

How to Run

Place finaldata.txt in the same folder as the notebook.

Open coders-of-bangalore (1).ipynb in Jupyter Notebook or JupyterLab.

Run the cells from top to bottom.

Review the outputs for maximum posts, maximum followers, maximum following, and categories.

Project Objective

The project demonstrates basic Python data processing concepts including:

File handling

String manipulation

Splitting raw text into records

Lists

Dictionaries

Sets

Functions

Loops

Conditional statements

Data cleaning

Finding maximum values