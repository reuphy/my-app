}); it('should test the value of the BehaviorSubject', (done) => { const mockData1 = { key1: 'value1' }; const mockData2 = { key2: 'value2' }; spyOn(dataService, 'getData1').and.returnValue(of(mockData1)); spyOn(dataService, 'getData2').and.returnValue(of(mockData2)); spyOn(dataService, 'processData').and.callThrough(); component.fetchData(); dataService.getDataSubject().subscribe(value => { expect(value).toEqual({ key1: 'VALUE1', key2: 'VALUE2' }); done(); }); expect(dataService.processData).toHaveBeenCalled(); }); 
---------------------------
<h2>Modals</h2>
<hr>
<div class="row">
  <div class="col-lg-2 col-md-4 pb-md-0 pb-2">
    <button class="tk-btn tk-btn-secondary" (click)="displayModal = !displayModal;">Show</button>
  </div>
  <div class="col-lg-2 col-md-4 pb-md-0 pb-2">
    <h3>Modal</h3>
  </div>
  <!-- <div class="col-lg-8 col-md-4">
    -->
</div>
<hr>

<tk-modal 
  class="tk-modal-component" 
  [display]="displayModal" 
  [closeOutSideAllowed]="false" 
  [closeText]="'Close'" 
  (closeModal)="displayModal = false">
  Body
</tk-modal>


<div class="row">
  <div class="col-lg-2 col-md-4 pb-md-0 pb-2">
    <button class="tk-btn tk-btn-secondary" (click)="displayModal4 = !displayModal4;">Show</button>
  </div>
  <div class="col-lg-2 col-md-4 pb-md-0 pb-2">
    <h3>Modal</h3>
    Full width
  </div>
  <!-- <div class="col-lg-8 col-md-4">
   
  </div> -->
</div>
<hr>

<tk-modal 
  class="tk-modal-component" 
  [grid]="'col-lg-12 h-100 m-0'" 
  [display]="displayModal4" 
  [closeText]="'Close'" 
  (closeModal)="displayModal4 = false">
  Body
</tk-modal>

--------------------------
  public displayModal = false;
  public displayModal2 = false;
  public displayModal3 = false;
  public displayModal4 = false;
  ---------------------------------------------
  tk-modal html
  <div class="tk-modal {{ overflow }}" *ngIf="isModalDisplayed">
  <div class="tk-modal-background {{ backgroundcolor }}" (click)="closeOutSideAllowed ? close() : '';"></div>
  <div class="tk-modal-grid {{ grid }}">
    <div class="row tk-modal-content p-0 m-0">
      <div *ngIf="isTopRightCancelBtnDisplayed" class="tk-modal-header m-0">
        <button class="tk-btn tk-btn-icon tk-btn-no-border tk-btn-modal-close" (click)="close();">
          <span class="tk-btn-icon-icon">
            <i class="icon icon-close" aria-hidden="true"></i>
          </span>
          <span *ngIf="closeText" class="close-text">{{ closeText }}</span>
        </button>
      </div>
      <div class="col-12 tk-modal-body p-3 p-md-4">
        <ng-content></ng-content>
      </div>
    </div>
  </div>
</div>
------------------------------------
css
// --tk-color-primary-white: #FFFFFF;

/* MODAL STYLES
-------------------------------*/
.tk-modal-component {
  /* modals are hidden by default */
  display: none;

  .tk-modal {
    /* modal container fixed across whole screen */
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: z-index('modal');
    overflow: auto;

    .tk-modal-grid {
      position: relative;
      background: #FFFFFF;
      min-height: 100px;
      margin: auto;
      margin-top: 30px;
      margin-bottom: 30px;

      .tk-modal-content {
        .tk-modal-body {
          padding: 22px;
        }
      }
    }

    // @media (max-width: map_get($grid-breakpoints, md)) {
      .tk-modal-grid {
        width: 100%;
        margin: auto;
      }
    // }
  }

  .tk-modal-background {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: black;
    opacity: 0.75;
    z-index: -1;
    cursor: pointer;
  }
}

.tk-modal-center {
  .tk-modal {
    display: flex;
  }
}

.tk-modal-header {
  position: absolute;
  right: 0;
  padding-top: 12px; // tk-btn-icon has already 12px padding top => 24px
  z-index: 1;
  // @media (max-width: map_get($grid-breakpoints, md)) {
  //   padding-top: 4px;
  // }
}

.tk-btn.tk-btn-modal-close {
  padding: 0 14px 0 0;
  flex-direction: column;
  // Note: the close icon is not following icon design guidelines but
  // this is what designers want
  .tk-btn-icon-icon {
    height: 24px;
    width: 24px;
    padding: 5px;

    .icon {
      height: 14px;
      width: 14px;
      font-size: 14px;
    }
  }
  .close-text {
    margin-top: 4px;
    font-family: font-family('csePRoman');
    font-size: 12px;
    line-height: 16px;
    // @media (max-width: map_get($grid-breakpoints, md)) {
    //   display: none;
    // }
  }
}
-------------------------------
ts
import { Component, OnInit, Input, Output, EventEmitter, ElementRef, OnDestroy } from '@angular/core';

@Component({
  selector: 'tk-modal',
  templateUrl: './tk-modal.component.html'
})
export class TkModalComponent implements OnInit, OnDestroy {
  isModalDisplayed: boolean = false;
  @Input() setOverlay101Vh = false;
  @Input()
  set display(value: boolean) {
    value ? this.open() : this.close();
    this.isModalDisplayed = value;
  }
  @Input() closeOutSideAllowed = true;

  // Note: you can only manage col-md grid or higher dimension since, with lower dimensions (sm), the modal takes full screen width.
  @Input() grid: string = 'col-lg-8 col-md-10'; // default value
  @Input() backgroundcolor: string = ''; // default value
  @Input() overflow: string = 'auto'; // default value
  @Input() closeText?: string;
  @Input() isTopRightCancelBtnDisplayed: boolean = true;
  @Input() isStackModalInput: boolean = false;
  @Output() closeModal = new EventEmitter<boolean>();
  bodyTopPosition = '';
  element: any;
  constructor(private el: ElementRef) {
    this.element = el.nativeElement;
  }

  ngOnInit() {
    // This code will add modal at the end of body elements
    // Needed to pass over other elements, like header
  }

  ngOnDestroy() {
    // We remove the modal from body's children
    this.close();
  }

  open() {
    document.body.appendChild(this.element);
    this.bodyTopPosition = `-${window.scrollY}px`;
  //  document.body.style.position = 'fixed';
    this.element.style.display = 'block';
    if(this.setOverlay101Vh)
      document.body.classList.add('tk-modal-open-101vh');
      else
      document.body.classList.add('tk-modal-open');
    
  }

  close() {
    if (this.element.style.display === 'block' && !this.isStackModalInput) {
      this.element.style.display = 'none';
      const scrollY = this.bodyTopPosition;
    if(this.setOverlay101Vh)
      document.body.classList.remove('tk-modal-open-101vh');
      else
      document.body.classList.remove('tk-modal-open');
      this.closeModal.emit();
      document.body.style.position = '';
      window.scrollTo(0, parseInt(scrollY || '0') * -1);
      document.body.removeChild(this.element);
    }
      else 
      this.closeModal.emit();
  }
}
------------------------------
body.tk-modal-open {
  height: 100vh;
  overflow-y: scroll;
} 
