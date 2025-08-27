# @uthayakumar-dinesh/statusify

[![npm version](https://badge.fury.io/js/@uthayakumar-dinesh%2Fstatusify.svg)](https://badge.fury.io/js/@uthayakumar-dinesh%2Fstatusify)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/%3C%2F%3E-TypeScript-%230074c1.svg)](http://www.typescriptlang.org/)

A TypeScript-first npm package that provides HTTP status codes as enums and literal types, with utility functions for reverse lookup operations. Perfect for building type-safe web applications and APIs.

## Features

✨ **Type-Safe HTTP Status Codes**: All HTTP status codes available as uppercase enum values
🔍 **Reverse Lookup**: Helper function to get status name from numeric code
📝 **Full TypeScript Support**: Complete type definitions and IntelliSense support
🎯 **Zero Dependencies**: Lightweight package with no external dependencies
🚀 **Tree-Shakeable**: Only import what you need for optimal bundle size

## Installation

```bash
npm install @uthayakumar-dinesh/statusify
```

```bash
yarn add @uthayakumar-dinesh/statusify
```

```bash
pnpm add @uthayakumar-dinesh/statusify
```

## Usage

### Basic Usage

```typescript
import { HTTP_STATUS, getStatusName } from "@uthayakumar-dinesh/statusify";

// Using enum values
console.log(HTTP_STATUS.OK); // 200
console.log(HTTP_STATUS.NOT_FOUND); // 404
console.log(HTTP_STATUS.INTERNAL_SERVER_ERROR); // 500

// Reverse lookup - get status name from code
console.log(getStatusName(200)); // "OK"
console.log(getStatusName(404)); // "NOT_FOUND"
console.log(getStatusName(500)); // "INTERNAL_SERVER_ERROR"
```

### Express.js Example

```typescript
import express from "express";
import { HTTP_STATUS, getStatusName } from "@uthayakumar-dinesh/statusify";

const app = express();

app.get("/users/:id", (req, res) => {
  const user = findUser(req.params.id);

  if (!user) {
    return res.status(HTTP_STATUS.NOT_FOUND).json({
      error: "User not found",
      statusCode: HTTP_STATUS.NOT_FOUND,
      statusName: getStatusName(HTTP_STATUS.NOT_FOUND),
    });
  }

  res.status(HTTP_STATUS.OK).json(user);
});
```

### API Response Helper

```typescript
import { HTTP_STATUS, getStatusName } from "@uthayakumar-dinesh/statusify";

function createApiResponse<T>(
  statusCode: HTTP_STATUS,
  data?: T,
  message?: string,
) {
  return {
    statusCode,
    statusName: getStatusName(statusCode),
    success: statusCode >= 200 && statusCode < 300,
    message,
    data,
  };
}

// Usage
const successResponse = createApiResponse(
  HTTP_STATUS.CREATED,
  { id: 1, name: "John" },
  "User created successfully",
);
const errorResponse = createApiResponse(
  HTTP_STATUS.BAD_REQUEST,
  null,
  "Invalid input data",
);
```

## API Reference

### HTTP_STATUS Enum

The main enum containing all HTTP status codes as uppercase constants.

```typescript
enum HTTP_STATUS {
  // 1xx Informational
  CONTINUE = 100,
  SWITCHING_PROTOCOLS = 101,

  // 2xx Success
  OK = 200,
  CREATED = 201,
  // ... and more
}
```

### HTTP_STATUS_LITERAL Type

A literal type union of all available HTTP status codes for enhanced type safety.

```typescript
type HTTP_STATUS_LITERAL = 100 | 101 | 200 | 201 | 202 | ... // all status codes
```

### getStatusName Function

Returns the enum key name for a given HTTP status code.

```typescript
function getStatusName(code: HTTP_STATUS_LITERAL): string;
```

**Parameters:**

- `code` (HTTP_STATUS_LITERAL): The numeric HTTP status code

**Returns:**

- `string`: The uppercase enum key name corresponding to the status code

**Example:**

```typescript
getStatusName(200); // Returns: "OK"
getStatusName(404); // Returns: "NOT_FOUND"
getStatusName(500); // Returns: "INTERNAL_SERVER_ERROR"
```

## Available HTTP Status Codes

### 1xx Informational

| Code | Enum Key              | Description         |
| ---- | --------------------- | ------------------- |
| 100  | `CONTINUE`            | Continue            |
| 101  | `SWITCHING_PROTOCOLS` | Switching Protocols |

### 2xx Success

| Code | Enum Key                        | Description                   |
| ---- | ------------------------------- | ----------------------------- |
| 200  | `OK`                            | OK                            |
| 201  | `CREATED`                       | Created                       |
| 202  | `ACCEPTED`                      | Accepted                      |
| 203  | `NON_AUTHORITATIVE_INFORMATION` | Non-Authoritative Information |
| 204  | `NO_CONTENT`                    | No Content                    |
| 205  | `RESET_CONTENT`                 | Reset Content                 |
| 206  | `PARTIAL_CONTENT`               | Partial Content               |

### 3xx Redirection

| Code | Enum Key             | Description        |
| ---- | -------------------- | ------------------ |
| 300  | `MULTIPLE_CHOICES`   | Multiple Choices   |
| 301  | `MOVED_PERMANENTLY`  | Moved Permanently  |
| 302  | `FOUND`              | Found              |
| 303  | `SEE_OTHER`          | See Other          |
| 304  | `NOT_MODIFIED`       | Not Modified       |
| 307  | `TEMPORARY_REDIRECT` | Temporary Redirect |
| 308  | `PERMANENT_REDIRECT` | Permanent Redirect |

### 4xx Client Errors

| Code | Enum Key                 | Description            |
| ---- | ------------------------ | ---------------------- |
| 400  | `BAD_REQUEST`            | Bad Request            |
| 401  | `UNAUTHORIZED`           | Unauthorized           |
| 402  | `PAYMENT_REQUIRED`       | Payment Required       |
| 403  | `FORBIDDEN`              | Forbidden              |
| 404  | `NOT_FOUND`              | Not Found              |
| 405  | `METHOD_NOT_ALLOWED`     | Method Not Allowed     |
| 406  | `NOT_ACCEPTABLE`         | Not Acceptable         |
| 408  | `REQUEST_TIMEOUT`        | Request Timeout        |
| 409  | `CONFLICT`               | Conflict               |
| 410  | `GONE`                   | Gone                   |
| 411  | `LENGTH_REQUIRED`        | Length Required        |
| 412  | `PRECONDITION_FAILED`    | Precondition Failed    |
| 413  | `PAYLOAD_TOO_LARGE`      | Payload Too Large      |
| 414  | `URI_TOO_LONG`           | URI Too Long           |
| 415  | `UNSUPPORTED_MEDIA_TYPE` | Unsupported Media Type |
| 429  | `TOO_MANY_REQUESTS`      | Too Many Requests      |

### 5xx Server Errors

| Code | Enum Key                | Description           |
| ---- | ----------------------- | --------------------- |
| 500  | `INTERNAL_SERVER_ERROR` | Internal Server Error |
| 501  | `NOT_IMPLEMENTED`       | Not Implemented       |
| 502  | `BAD_GATEWAY`           | Bad Gateway           |
| 503  | `SERVICE_UNAVAILABLE`   | Service Unavailable   |
| 504  | `GATEWAY_TIMEOUT`       | Gateway Timeout       |

## TypeScript Usage

This package is built with TypeScript and provides full type safety:

```typescript
import {
  HTTP_STATUS,
  HTTP_STATUS_LITERAL,
  getStatusName,
} from "@uthayakumar-dinesh/statusify";

// Type-safe status code usage
function handleResponse(status: HTTP_STATUS_LITERAL) {
  switch (status) {
    case HTTP_STATUS.OK:
      return "Success!";
    case HTTP_STATUS.NOT_FOUND:
      return "Resource not found";
    case HTTP_STATUS.INTERNAL_SERVER_ERROR:
      return "Server error";
    default:
      return `Unknown status: ${getStatusName(status)}`;
  }
}

// IntelliSense will show all available status codes
const myStatus: HTTP_STATUS_LITERAL = HTTP_STATUS.CREATED;
```

## Contributing

We welcome contributions! Please follow these steps:

1. **Fork the repository**

   ```bash
   git clone git@github.com:Dineshs737/statusify.git
   ```

2. **Create a feature branch**

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Add tests for any new functionality
   - Ensure all tests pass: `npm test`
   - Follow the existing code style

4. **Commit your changes**

   ```bash
   git commit -m "feat: add your feature description"
   ```

5. **Push to your fork and create a Pull Request**

### Development Setup

```bash
# Clone the repository
git git@github.com:Dineshs737/statusify.git
cd statusify

# Install dependencies
npm install

# Run tests
npm test

# Build the project
npm run build
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Repository

- **GitHub**: [https://github.com/uthayakumar-dinesh/statusify](https://github.com/Dineshs737/statusify)
- **npm**: [@uthayakumar-dinesh/statusify](https://www.npmjs.com/package/@uthayakumar-dinesh/statusify)

---

Made with ❤️ by [Uthayakumar Dinesh](https://github.com/Dineshs737)
