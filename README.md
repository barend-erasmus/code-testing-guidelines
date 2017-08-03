# Code Testing Guidelines

Unit Test, Integration Test, Functional Test, Acceptance Test

## Unit Test

Unit Test should only isolated methods which is does not have dependencies on any external resources.

If the method is using a external resources such as file system, network or database, it's not a unit test.

```javascript

// Method
function isValidDateFormat(dateString) {
    const pattern = new RegExp(/^\d{2}([./-])\d{2}\1\d{4}$/);
    return pattern.test(dateString);
}

// Unit Test
describe('isValidDateFormat', () => {
    it('should return false given invalid date format', () => {
        const result = isValidDateFormat('2017-01-01');
        expect(result).to.be.false;
    });

    it('should return true given valid date format', () => {
        const result = isValidDateFormat('01-01-2017');
        expect(result).to.be.true;
    });
});

```

## Integration Test

Integration Test should test the integration of your code with external resources. 

```javascript

// Method
function getDateFromServer() {
    return request('http://my-date-server.local/current-date')
        .then((result) => {
            return JSON.parse(result);
        });
}

// This method should be Unit Tested
function parseResultoDate(result) {
    try {
        return JSON.parse(result).currentDate;
    }catch(err) {
        return null;
    }
}

// Integration Test
describe('getDateFromServer', () => {
    it('should return date', () => {
        return getDateFromServer().then((result) => {
            expect(result).to.be.not.null;
        });
    });
});

```

## Functional Test

Coming Soon...

## Acceptance Test

Coming Soon...