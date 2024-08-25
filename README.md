app.module.ts

  providers: [
   isMock ? fakeBackendProvider : [],
   isMock2 ? fakeBackendProvider2 : [],
  ],
  
------------------------------
fakebackend.ts

import { HTTP_INTERCEPTORS, HttpEvent, HttpHandler, HttpInterceptor, HttpRequest, HttpResponse } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { delay, Observable, of } from "rxjs";
import { getMockResponse } from "./mock.helper";

@Injectable()
export class FakeBackendHttpInterceptor implements HttpInterceptor {
   intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
     return this.handleRequests(req, next);
   }
 
   handleRequests(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
     const { url } = req;
     const mockResponse = getMockResponse(url);
     if (mockResponse) {
       return of(new HttpResponse({ status: 200, body: mockResponse })).pipe(delay(500));
     }
     return next.handle(req);
   }
 }

export const fakeBackendProvider = {
   provide: HTTP_INTERCEPTORS,
   useClass: FakeBackendHttpInterceptor,
   multi: true,
};

-------------------------------
mock.helper.ts

export function getMockResponse(url: string): any {
 
  const mock = {
    "userId": 1,
    "id": 1,
    "title": "test",
    "completed": false
  };

    if (url === 'https://jsonplaceholder.typicode.com/todos/1') {
      return mock;
    }
    return null;
  }
  -------------------------------
