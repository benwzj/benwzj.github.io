---
layout: post
title: YAML Introduce
date: 2020-07-10
tags: YAML JSON
category: Language
---

## What is YAML

YAML (a recursive acronym for "YAML Ain't Markup Language") is a human-readable data-serialization language. It is commonly used for `configuration files` and in applications where data is being stored or transmitted.

Custom data types are allowed, but YAML natively encodes scalars (such as strings, integers, and floats), lists, and associative arrays (also known as maps, dictionaries or hashes). 

### Basic Syntax
1. Whitespace indentation is used for denoting structure;
2. Comments begin with the number sign (#)
3. List members are denoted by a leading hyphen (-) with one member per line.
4. An associative array entry is represented using colon space in the form key: value with one entry per line. YAML requires the colon be followed by a space!
5. Strings (one type of scalar in YAML) are ordinarily unquoted, but may be enclosed in double-quotes ("), or single-quotes (').
6. Block scalars are delimited with indentation with optional modifiers to preserve (|) or fold (>) newlines.
7. Multiple documents within a single stream are separated by three hyphens (---).
......

### Basic Example
```yaml
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-a4c7edb2
      InstanceType: t2.micro
```

## YAML Array 

Yaml Array is a bit tricky, especially the nested array.

### Array of scalar items 

With key: 
```yaml
key1:
  - value1
  - value2
  - value3
  - value4
  - value5
```
equal to JSON:
```json
{
  "key1": [
    "value1",
    "value2",
    "value3",
    "value4",
    "value5"
  ]
}
```

Without key:
```yaml
- value1
- value2
- value3
- value4
- value5
```
Which equal to JSON:
```json
[
  "value1",
  "value2",
  "value3",
  "value4",
  "value5"
]
```

### Array for object
```yaml
one:
  - id: 1
    name: franc
  - id: 11
    name: Tom
```
Equal to JSON:
```json
{
  "one": [
    {
      "id": 1,
      "name": "franc"
    },
    {
      "id": 11,
      "name": "Tom"
    }
  ]
}
```

### YAML Nested Array
```yaml
employees:
  -
    id: 213
    name: franc
    others:
      - { department: sales, did: 1}
      - { salary: 5000}
      - { address: USA, pincode: 97845 }
```
Equal to :
```json
{
  "employees": [
    {
      "id": 213,
      "name": "franc",
      "others": [
        {
          "department": "sales",
          "did": 1
        },
        {
          "Salary": 5000
        },
        {
          "address": "USA",
          "pincode": 97845
        }
      ]
    }
  ]
}
```
```yaml
- name: State Rules
- name: useState Hook
- - name: Syntax
  - name: How React implement Hook
- name: FQA
```
equal to:
```json
[
  {"name":"State Rules"}, 
  {"name": "useState Hook"}, 
  [ {"name":"Syntax"}, {"name":"How React implement Hook"}],
  {"name":"FQA"}
]
```

## YAML has been criticized 
for its significant whitespace, confusing features, insecure defaults, less secure, and its complex and ambiguous specification:
- Configuration files can execute commands or load contents without the users realizing it.
- Editing large YAML files is difficult, as indentation errors can go unnoticed.
- Type autodetection is a source of errors. For example, unquoted Yes and NO are converted to booleans; software version numbers might be converted to floats.
- Truncated files are often interpreted as valid YAML due to the absence of terminators.
- The complexity of the standard led to inconsistent implementations and making the language non-portable.
