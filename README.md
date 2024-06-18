import { ComponentFixture, TestBed } from '@angular/core/testing';
import { DeleteComponent } from './delete.component';
import { DeleteService } from './delete.service';

describe('DeleteComponent', () => {
  let component: DeleteComponent;
  let fixture: ComponentFixture<DeleteComponent>;
  let deleteService: DeleteService;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [DeleteComponent],
      providers: [DeleteService]
    });
    fixture = TestBed.createComponent(DeleteComponent);
    component = fixture.componentInstance;
    deleteService = TestBed.inject(DeleteService);
  });

  it('should delete record from grid on click of OK', () => {
    spyOn(window, 'confirm').and.returnValue(true);
    const mockId = 'some-id'; // Remplacez par un ID réel
    spyOn(deleteService, 'deleteRecord').and.returnValue({ subscribe: () => {} });
    component.deleteRecord(mockId);
    expect(deleteService.deleteRecord).toHaveBeenCalledWith(mockId);
  });

  it('should not delete record from grid on click of Cancel', () => {
    spyOn(window, 'confirm').and.returnValue(false);
    const mockId = 'some-id'; // Remplacez par un ID réel
    spyOn(deleteService, 'deleteRecord');
    component.deleteRecord(mockId);
    expect(deleteService.deleteRecord).not.toHaveBeenCalled();
  });
});
