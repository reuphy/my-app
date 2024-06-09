# my-app
angular project
const filterArrayByMatchingProperty = (
    array: { [key: string]: string }[], 
    arrayTofilter: { [key: string]: string }[], 
    prop: string) => {
    return arrayTofilter.filter((item) => {
        return array.some((element) => {
            return element[prop] === item[prop];
        });
    });         
}

// make some tests
const array = [
    { id: '1', name: 'John' },
    { id: '2', name: 'Doe' },
    { id: '3', name: 'Jane' }
];

const arrayToFilter = [
    { id: '1', name: 'John' },
    { id: '3', name: 'Jane' }
];

const result = filterArrayByMatchingProperty(array, arrayToFilter, 'id');
console.log(result); // [{ id: '1', name: 'John' }, { id: '3', name: 'Jane' }]

// js-helpers.spec.ts
// import { filterArrayByMatchingProperty } from './js-helpers';

// describe('filterArrayByMatchingProperty', () => {
//   it('should return items with matching properties', () => {
//     const array = [
//       { id: '1', name: 'John' },
//       { id: '2', name: 'Doe' },
//       { id: '3', name: 'Jane' }
//     ];
//     const arrayToFilter = [
//       { id: '1', name: 'John' },
//       { id: '3', name: 'Jane' }
//     ];
//     const prop = 'id';
//     const result = filterArrayByMatchingProperty(array, arrayToFilter, prop);
//     expect(result).toEqual([
//       { id: '1', name: 'John' },
//       { id: '3', name: 'Jane' }
//     ]);
//   });

//   it('should return an empty array if no properties match', () => {
//     const array = [
//       { id: '1', name: 'John' },
//       { id: '2', name: 'Doe' },
//       { id: '3', name: 'Jane' }
//     ];
//     const arrayToFilter = [
//       { id: '4', name: 'John' },
//       { id: '5', name: 'Jane' }
//     ];
//     const prop = 'id';
//     const result = filterArrayByMatchingProperty(array, arrayToFilter, prop);
//     expect(result).toEqual([]);
//   });
// });

